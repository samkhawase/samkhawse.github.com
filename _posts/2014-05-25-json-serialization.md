---
layout: post
title: "Json Serialization"
category: tech
tags: [iOS, programming]
---
{% include JB/setup %}

[Json](http://json.org/) i.e. JavaScript Open Notation is a lightweight, human readable and machine parsable data format. Combined with the RESTful API respresentation, it is by far the most popular data exchange format for mobile apps. Given it's ease of use, extensibility and speed of parsing, it trumps over the age old XML format. 

JSON is built on two structures: A collection of name/value pairs (E.g. Objects) or an ordered list of values (E.g. Lists, Arrays). The canonical `Person` object is described in the following manner in JSON.

	{
	    "firstName": "Max",
	    "lastName": "Mustermann",
	    "age": 25,
	    "address": {
	        "streetAddress": "Zimmerstr. 22",
	        "city": "Berlin",
	        "state": "Berlin",
	        "postalCode": "10117"
	    },
	    "phoneNumber": [
	        {
	            "type": "home",
	            "number": "030/22334455"
	        },
	        {
	            "type": "mobile",
	            "number": "0176/22334455"
	        }
	    ],
	    "gender":{
	         "type":"male"
	    }
	}

In iOS the JSON parsing is done using NSJSONSerialization class, which converts json strings to Foundation objects and vice versa. There are several rules that need to be followed for the serialization/deserialization

* The top level object is an NSArray or NSDictionary.

* All objects are instances of NSString, NSNumber, NSArray, NSDictionary, or NSNull.

* All dictionary keys are instances of NSString.

* Numbers are not NaN or infinity.

The best way to test it is calling [isValidJSONObject:](https://developer.apple.com/library/ios/documentation/foundation/reference/nsjsonserialization_class/Reference/Reference.html#//apple_ref/occ/clm/NSJSONSerialization/isValidJSONObject:).

The NSJSONSerialization class provides static methods to create and parse JSON data. The method `dataWithJSONObject` retuns JSON data from a Foundation object. It's syntax is:

	+ (NSData *)dataWithJSONObject:(id)obj
			       options:(NSJSONWritingOptions)opt 
				 error:(NSError **)error

The parameters are: 

* `obj`:The object from which to generate JSON data. Must not be nil. Must be an 
* `opt`:Options for creating the JSON data mentioned under `NSJSONWritingOptions`.
* `error`: an NSError object that describes the problem in case of an error.

The method `writeJSONObject` is another way to get JSON data from Foundation objects. It writes a given JSON object to a provided stream. It returns the number of bytes written or 0 in case of error. It's syntax is: 

	+ (NSInteger)writeJSONObject:(id)obj 
			toStream:(NSOutputStream *)stream 
			options:(NSJSONWritingOptions)opt 
			error:(NSError **)error


It has the following parameters:

* `obj`: The object to write to stream.
* `stream`: The stream to which to write.
* `opt`: Options for writing the JSON data listed under `NSJSONWritingOptions`.
* `error`: an NSError object that describes the problem in case of an error.

The class method to deserialize a JSON string is `JSONObjectWithData`  which returns a Foundation object from given JSON data.

	+ (id)JSONObjectWithData:(NSData *)data 
			 options:(NSJSONReadingOptions)opt 
			   error:(NSError **)error


The parameters are:

* data: A data object containing JSON data.
* opt: Options for reading the JSON data and creating the Foundation objects listed under `NSJSONReadingOptions`.
* error: If an error occurs, upon return contains an NSError object that describes the problem.


Another way to get Foundation objects from JSON data is `JSONObjectWithStream`. It returns a Foundation object from JSON data in a given stream.

	+ (id)JSONObjectWithStream:(NSInputStream *)stream 
				options:(NSJSONReadingOptions)opt 
				error:(NSError **)error

It has the following parameters:

* `stream`: A stream from which to read JSON data. 
* `opt`: Options for reading the JSON data and creating the Foundation objects listed under `NSJSONReadingOptions`.
* `error`: If an error occurs, upon return contains an NSError object that describes the problem.
