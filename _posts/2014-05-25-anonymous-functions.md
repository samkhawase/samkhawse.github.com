---
layout: post
title: "Anonymous Functions"
category: 
tags: []
---
{% include JB/setup %}

Anonymous functions or Lambdas are one of the most desirable features of a language. Languages like Erlang, C#, Golang, Python etc use lambdas to make life easy for developers. Unfortunately the language I worked with most of the time, Java, didn't have any support for lambdas. When I came across the beauty of lambdas, it completely changed the way I used to write programs.

Lambdas in objective-c are called `blocks` and they are first order functions. First order functions are treated like any other object and can be passed around in methods and can be returned from it. 

The basic block syntax is:

	returnType (^blockName)(parameterTypes) = ^returnType(parameters) { //block body };

The following code declares a block with the name `areaCalculatorBlock` which returns a `double` and takes two parameters of type `double`

	double (^areaCalculatorBlock)(double, double) = ^double(double height, double width){
		return height * width;
	}

Blocks support closure, i.e. they capture values from enclosing scope. In the following code excerpt, the `outerHeight` and `outerWidth` are available to the block `areaCalculatorBlock`

	- (void)aMethod {

		double outerHeight = 100;
    	double outerWidth = 200;
    
    	double (^areaCalculatorBlock)(void) = ^double(void){
			return outerHeight * outerWidth;
		}

    	NSLog("area is: %ld", areaCalculatorBlock());
	}

Blocks can use as a shared variable using the `__block` syntax. In the following block `sharedHeight` and `sharedWidth` are readily available inside the block.

	__block double sharedHeight = 100;
	__block double sharedWidth = 200;

	- (void)aMethod{
		
		double (^areaCalculatorBlock)(void) = ^double(void){
			return sharedHeight * sharedWidth;
		}

		NSLog("area is: %ld", areaCalculatorBlock());
	}

Blocks are most prominently used as parameters to functions. This mechanism allows blocks to be used as callbacks that define logic to be executed when the task completes. Grand Central Dispatch uses this extensively to handle IBAction code.

An example from Apple's documentation shows it in action. In the following code, once the 'task' is finished, it calls the block to hide the progress indicator. 

	- (IBAction)fetchRemoteInformation:(id)sender {
	    [self showProgressIndicator];

	    RandomWebTask *task = // Code to define the RandonWebTask
	    
	    [task beginTaskWithCallbackBlock:^{
	        [self hideProgressIndicator];
	    }];
	}

Blocks are a wonderful way of handling asynchronous code and make it really easy to handle callbacks.

[Apple's Developer Documentation](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/WorkingwithBlocks/WorkingwithBlocks.html) is a great resource to dive in the world of blocks.

