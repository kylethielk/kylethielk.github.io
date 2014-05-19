---
author: Kyle Thielk
comments: true
date: 2014-02-07 18:27:11+00:00
layout: post
slug: objective-c-instantiate-child-class-from-parent
title: 'Objective-C: Instantiate Child Class from Parent'
wordpress_id: 1014
categories:
- Tech
---

A common problem encountered (in any language) is how to instantiate a child class from an instance of a base parent in a generic way.  In Objective-C take for example:



{% codeblock lang:obj-c %}
@interface Vehicle : NSObject

@property (copy,nonatomic) NSString *name; 

@end

@interface Motorcycle : Vehicle

@property (copy,nonatomic) NSNumber *engineCC;

@end
{% endcodeblock %}

If we have an instance of Vehicle and we want to get an instance of a Motorcycle we could manually copy each property:

**Manual Property Copy**

{% codeblock lang:obj-c %}
Vehicle *vehicle = [Vehicle new];
vehicle.name = @"MyVehicle";

Motorcycle *motorcycle = [Motorcycle new];
motorcycle.name = vehicle.name;
motorcycle.engineCC = [NSNumber numberWithInt:200];
{% endcodeblock %}

Seems simple enough in this case, but when the property count starts increasing so does the amount of typing. If only there were a better way...:

{% codeblock lang:obj-c %}
+ (void) copyParent:(id)parent intoChild:(id) child
{
    id parentClass = [parent class];
    NSString *propertyName;
    unsigned int outCount, i;

    //Get a list of properties for the parent class.
    objc_property_t *properties = class_copyPropertyList(parentClass, &outCount);

    //Loop through the parents properties.
    for (i = 0; i < outCount; i++)
    {
        objc_property_t property = properties[i];

        //Convert the property to a string.
        propertyName = [NSString stringWithCString:property_getName(property) encoding:NSASCIIStringEncoding];

        //Get the parent's value for the property
        id value = [parent valueForKey:propertyName];

        //..and copy into the child.
        if ([value conformsToProtocol:@protocol(NSCopying)])
        {
            [child setValue:[value copy] forKey:propertyName];
        }
        else
        {
            [child setValue:value forKey:propertyName];
        }

    }
}
{% endcodeblock %}

_Note: Make sure to import objc/runtime.h:_

{% codeblock lang:obj-c %}
#import <objc/runtime.h>
{% endcodeblock %}

Sample Usage (assuming we put the method in a class called Utils):

{% codeblock lang:obj-c %}
Vehicle *vehicle = [Vehicle new];
vehicle.name = @"MyVehicle";

Motorcycle *motorcycle = [Motorcycle new];
[Utils copyParent:vehicle intoChild:motorcycle];
{% endcodeblock %}
