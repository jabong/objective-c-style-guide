# Rocket Internet Objective-C Style Guide

This style guide outlines the coding conventions of the iOS development at Rocket Internet. We welcome your feedback in [issues](https://github.com/rocketinternet/objetive-c-style-guide/issues) and [pull requests](https://github.com/rocketinternet/objetive-c-style-guide/pulls). Also, [we're hiring](http://www.rocket-internet.de/jobs).

**Please also see our emerging [Guidelines](/Guidelines/README.md) section for Rocket-style implementations and best practices.**

When in doubt about a particular implementation, either you state it public in an issue, or fall back to the recommendations in Apple's [Coding Guidelines for Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html).

Thanks to all of [our contributors](https://github.com/rocketinternet/objective-c-style-guide/contributors).

## Introduction

Here are some of the documents from Apple that informed the style guide. If something isn't mentioned here, it's probably covered in great detail in one of these:

* [Coding Guidelines for Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
* [The Objective-C Programming Language](http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
* [Cocoa Fundamentals Guide](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CocoaFundamentals/Introduction/Introduction.html)
* [iOS App Programming Guide](http://developer.apple.com/library/ios/#documentation/iphone/conceptual/iphoneosprogrammingguide/Introduction/Introduction.html)

## Table of Contents

* [Language](#language)
* [Xcode Project](#xcode-project)
* [Files](#files)
* [Code Organization](#code-organization)
* [Dot-Notation Syntax](#dot-notation-syntax)
* [Spacing](#spacing)
* [Conditionals](#conditionals)
    * [Chaining expressions inside conditionals](#chaining-expressions-inside-conditionals)
    * [Ternary Operator](#ternary-operator)
* [Error handling](#error-handling)
* [Selectors](#selecttors)
    * [Initialization Selectors](#initialization-selectors)
    * [Class constructor Selectors](#class-constructor-selectors)
    * [Private Selectors](#private-selectors)
    * [dealloc](#dealloc)
* [Variables](#variables)
    * [Properties](#private)
        * [Private Properties](#private-properties)
        * [Property Attributes](#property-attributes)
* [Naming](#naming)
* [Blocks](#blocks)
* [Protocols](#protocols)
* [Comments](#comments)
* [Literals](#literals)
* [CGRect Functions](#cgrect-functions)
* [Constants](#constants)
* [Enumerated Types](#enumerated-types)
* [Case Statements](#case-statements)
* [Golden Path](#golden-path)
* [Image Naming](#image-naming)
* [Booleans](#booleans)
* [Singletons](#singletons)

## Language

US English should be used.

**Preferred:**
```objc
UIColor *myColor = [UIColor whiteColor];
```

**Not Preferred:**
```objc
UIColor *myColour = [UIColor whiteColor];
```

## Xcode project

The physical files should be kept in sync with the Xcode project files in order to avoid file sprawl. Any Xcode groups created should be reflected by folders in the filesystem. Code should be grouped not only by type, but also by feature for greater clarity.

When possible, always turn on "Treat Warnings as Errors" in the target's Build Settings and enable as many [additional warnings](http://boredzo.org/blog/archives/2009-11-07/warnings) as possible. If you need to ignore a specific warning, use [Clang's pragma feature](http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas).

## Files

File names should reflect the name of the class implementation that they contain—including case. Follow the convention that your project uses.

File names for categories should include the name of the class being extended, e.g. GTMNSString+Utils.h or GTMNSTextView+Autocomplete.h

## Code Organization

Use `#pragma mark -` to categorize methods in functional groupings and protocol/delegate implementations following this general structure.

```objc
#pragma mark - Lifecycle

- (instancetype)init {}
- (void)dealloc {}
- (void)viewDidLoad {}
- (void)viewWillAppear:(BOOL)animated {}
- (void)didReceiveMemoryWarning {}

#pragma mark - Custom Accessors

- (void)setCustomProperty:(id)value {}
- (id)customProperty {}

#pragma mark - IBActions

- (IBAction)submitData:(id)sender {}

#pragma mark - Public

- (void)publicMethod {}

#pragma mark - Private

- (void)privateMethod {}

#pragma mark - Protocol conformance
#pragma mark - UITextFieldDelegate
#pragma mark - UITableViewDataSource
#pragma mark - UITableViewDelegate

#pragma mark - NSCopying

- (id)copyWithZone:(NSZone *)zone {}

#pragma mark - NSObject

- (NSString *)description {}
```

## Line Breaks

The maximum line length for Objective-C and Objective-C++ files is 100 columns. Projects may opt to use an 80 column limit for consistency with the C++ style guide.
You can make violations easier to spot by enabling Preferences > Text Editing > Page guide at column: 100 in Xcode. Long lines of code should be wrapped onto further lines. Wrapped lines should vertically align with the least preceding opening bracket.

**For example:**

```objc
self.productsRequest = [[SKProductsRequest alloc] 
                        initWithProductIdentifiers:productIdentifiers];
```

**Not:**
```objc
self.productsRequest = [[SKProductsRequest alloc] initWithProductIdentifiers:productIdentifiers];
```

When wrapping within a method call, all subsequent argument-value pairs should be placed on a single line and vertically align at the assignment colons.

**For example:**

```objc
[anyRandomObject runAsyncAction:productIdentifiers 
                    withOptions:options 
                        success:successBlock 
                        failure:failureBlock];
```

**Not:**

```objc
[anyRandomObject runAsyncAction:productIdentifiers withOptions:options success:successBlock failure:failureBlock];
```

or

```objc
[anyRandomObject runAsyncAction:productIdentifiers withOptions:options 
                        success:successBlock 
                        failure:failureBlock];
```

or 

```objc
[anyRandomObject runAsyncAction:productIdentifiers 
 withOptions:options 
 success:successBlock 
 failure:failureBlock];
```

## Dot-Notation Syntax

Dot-notation should **always** be used for accessing and mutating properties. Bracket notation is preferred in all other instances.

**For example:**
```objc
view.backgroundColor = [UIColor orangeColor];
[UIApplication sharedApplication].delegate;
```

**Not:**
```objc
[view setBackgroundColor:[UIColor orangeColor]];
UIApplication.sharedApplication.delegate;
```

## Spacing

* Indent using 4 spaces. Never indent with tabs. Be sure to set this preference in Xcode.
* Method braces and other braces (`if`/`else`/`switch`/`while` etc.) always open on the same line as the statement but close on a new line.

**For example:**
```objc
if (user.isHappy) {
    //Do something
}
else {
    //Do something else
}
```
* There should be exactly one blank line between methods to aid in visual clarity and organization. Whitespace within methods should separate functionality, but often there should probably be new methods.
* `@synthesize` and `@dynamic` should each be declared on new lines in the implementation.

## Conditionals

Conditional bodies should always use braces even when a conditional body could be written without braces (e.g., it is one line only) to prevent [errors](https://github.com/NYTimes/objective-c-style-guide/issues/26#issuecomment-22074256). These errors include adding a second line and expecting it to be part of the if-statement. Another, [even more dangerous defect](http://programmers.stackexchange.com/a/16530) may happen where the line "inside" the if-statement is commented out, and the next line unwittingly becomes part of the if-statement. In addition, this style is more consistent with all other conditionals, and therefore more easily scannable.

**For example:**
```objc
if (!error) {
    return success;
}
```

**Not:**
```objc
if (!error)
    return success;
```

or

```objc
if (!error) return success;
```

### Chaining expressions inside conditionals

Boolean expressions may be chained within conditionals. Chaining should not reach beyond a depth of 2 and should not span over multiple lines. If further depth levels are required you probably should consider a state machine instead of cumbersome if-else trains. 

Chaining should use parentheses even when the order of operations would do the same thing anyway, i.e. `(foo && bar) || baz` is preferred over `foo && bar || baz`. Obviously when operators do not change the applied order, parentheses should be omitted, i.e. `foo && bar && baz` is preferred over `(foo && bar) && baz` or `foo && (bar && baz)`.

**For example:**
```objc
if (self.isAnarchy || ((self.foo == bar) && self.isEditable)) {
    //Do something
}
```

**Not:**

```objc
if (self.isAnarchy || ([self.thing isKindOfClass:ImportantThing.class] && self.isEditable && self.isContentLocked)) {
    //Do something
}
```

or

```objc
if (self.isAnarchy || (self.foo == bar && self.isEditable && self.isContentLocked)) {
    //Do something
}
```

or 

```objc
if (self.isAnarchy 
    || (self.foo == bar && self.isEditable && self.isContentLocked)) {
    //Do something
}
```

or 

```objc

id doBlock = ^{
    //Do something
}

if (self.isAnarchy) {
    doBlock();
} else {
    BOOL isStateXAndEditable = (self.foo == bar && self.isEditable);
    BOOL canChangeContent = (isStateXAndEditable && self.isContentLocked);
    
    if (canChangeContent) {
        doBlock();
    }
}
```

Also consider performance gains from the order of the expressions, i.e. `foo || (bar && baz)` is executed faster than `(baz && baz) || foo`, since the expression will be evaluated from left to right.

### Ternary Operator

The Ternary operator, `?:` , should only be used when it increases clarity or code neatness. A single condition is usually all that should be evaluated. Evaluating multiple conditions is usually more understandable as an if statement, or refactored into instance variables.

**For example:**
```objc
result = a > b ? x : y;
```

**Not:**
```objc
result = a > b ? x = c > d ? c : d : y;
```

## Error handling

There should be no use of exceptions for flow control, but only to indicate programmer error. To indicate errors, use an `NSError **` argument to a method.

When methods return an error parameter by reference, their return value should be of boolean type. Then switch on the returned value, not the error variable.

**For example:**
```objc
NSError *error;
if (![self trySomethingWithError:&error]) {
    // Handle Error
}
```

**Not:**
```objc
NSError *error;
[self trySomethingWithError:&error];
if (error) {
    // Handle Error
}
```

Some of Apple’s APIs write garbage values to the error parameter (if non-NULL) in successful cases, so switching on the error can cause false negatives (and subsequently crash).

## Selectors

First of all, keep your class simple; avoid "kitchen-sink" APIs. If a selector doesn't need to be public, don't make it so. Use a private category to prevent cluttering the public header.

In selector signatures, there should be a space after the selector type (-/+ symbol). There should be a space between the selector segments (matching Apple's style). Always include a keyword and be descriptive with the word before the argument which describes the argument.

The usage of the word "and" is reserved.  It should not be used for multiple parameters as illustrated in the `initWithWidth:height:` example below.

**For example:**
```objc
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
- (void)sendAction:(SEL)aSelector to:(id)anObject forAllCells:(BOOL)flag;
- (id)viewWithTag:(NSInteger)tag;
- (instancetype)initWithWidth:(CGFloat)width height:(CGFloat)height;
```

**Not:**

```objc
-(void)setT:(NSString *)text i:(UIImage *)image;
- (void)sendAction:(SEL)aSelector :(id)anObject :(BOOL)flag;
- (id)taggedView:(NSInteger)tag;
- (instancetype)initWithWidth:(CGFloat)width andHeight:(CGFloat)height;
- (instancetype)initWith:(int)width and:(int)height;  // Never do this.
```

Selector invocations should be formatted much like selector declarations. There should be a space after the called object and a space each between the previous argument's value and the following argument name.

**For example:**
```objc
[label setExampleText:text image:image];
[superview taggedView:(NSInteger)tag];
```

**Not:**

```objc
[label setExampleText: text image: image];
[superview taggedView:(NSInteger) tag];
[superview taggedView: (NSInteger)tag];
[superview taggedView: (NSInteger) tag];
```

### Initialization Selectors

Initialization Selectors should follow the convention provided by Apple's generated code template.  A return type of `instancetype` should also be used instead of `id`. More information on instancetype can be found on [NSHipster.com](http://nshipster.com/instancetype/).

```objc
- (instancetype)init {
    self = [super init];
    if (self) {
        // ...
    }
    return self;
}
```

### Class Constructor Selectors

Where class constructor selectors are used, these should always return type of `instancetype` and never `id`. This ensures the compiler correctly infers the result type. 

```objc
@interface Airplane

+ (instancetype)airplaneWithType:(RWTAirplaneType)type;

@end
```

### Private selectors

As recommended in Apple's Coding Guidelines for Cocoa [here](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingMethods.html#//apple_ref/doc/uid/20001282-1003829), private selectors should use the class/namespace prefix, including an "_" underscore.

**For example:**

```objc
- (void)RI_privateSelector;
```

**Not:**

```objc
- (void)_privateSelector;
```

or 

```objc
- (void)__privateSelector;
```

or 

```objc
- (void)privateSelector;
```

### dealloc

`dealloc` methods, if any, should be placed at the top of the implementation, directly after the `@synthesize` and `@dynamic` statements. Init methods should be placed directly below the `dealloc` methods.


## Variables

Variables should be named as descriptively as possible. Single letter variable names should be avoided except in `for()` loops.

Asterisks indicating pointers belong with the variable.

**For example:**

```objc
NSString *text
```

**Not:**

```objc
NSString* text

NSString * text
```

### Properties

Property definitions should be used in place of naked instance variables whenever possible. Direct instance variable access should be avoided except in initializer methods (`init`, `initWithCoder:`, etc…), `dealloc` methods and within custom setters and getters. For more information on using Accessor Methods in Initializer Methods and dealloc, see [here](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6).

**For example:**

```objc
@interface RISection : NSObject

@property (nonatomic) NSString *headline;

@end
```

**Not:**

```objc
@interface RISection : NSObject {
    NSString *headline;
}
```

#### Private Properties

Private properties should be declared in class extensions (anonymous categories) in the implementation file of a class. Named categories (such as `RIPrivate` or `private`) should never be used unless extending another class.

**For example:**

```objc
@interface RIAdvertisement ()

@property (nonatomic, strong) GADBannerView *googleAdView;
@property (nonatomic, strong) ADBannerView *iAdView;
@property (nonatomic, strong) UIWebView *adXWebView;

@end
```

#### Property Attributes

Property attributes should be explicitly listed, and will help new programmers when reading the code.  The order of properties should be storage then atomicity, which is consistent with automatically generated code when connecting UI elements from Interface Builder.

**Preferred:**

```objc
@property (weak, nonatomic) IBOutlet UIView *containerView;
@property (strong, nonatomic) NSString *tutorialName;
```

**Not Preferred:**

```objc
@property (nonatomic, weak) IBOutlet UIView *containerView;
@property (nonatomic) NSString *tutorialName;
```

## Naming

Apple naming conventions should be adhered to wherever possible, especially those related to [memory management rules](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html) ([NARC](http://stackoverflow.com/a/2865194/340508)).

Long, descriptive method and variable names are good.

**For example:**

```objc
UIButton *settingsButton;
```

**Not**

```objc
UIButton *setBut;
```

A three letter prefix (e.g. `RI`) should always be used for class names and constants. Constants should be camel-case with all words capitalized and prefixed by the related class name for clarity.

**For example:**

```objc
static const NSTimeInterval RIArticleViewControllerNavigationFadeAnimationDuration = 0.3;
```

**Not:**

```objc
static const NSTimeInterval fadetime = 1.7;
```

Properties and local variables should be camel-case with the leading word being lowercase and should not be prefixed with underscores.

Instance variables should be camel-case with the leading word being lowercase, and should be prefixed with an underscore. This is consistent with instance variables synthesized automatically by LLVM. **If LLVM can synthesize the variable automatically, then let it.**

**For example:**

```objc
@synthesize descriptiveVariableName = _descriptiveVariableName;
```

**Not:**

```objc
id varnm;
```

## Blocks

Blocks provide a neat implementation for the implementation of asynchronous behavior. There should be a space between a block's return type and it's name. Block definitions should omit their return type when possible. Block definitions should omit their arguments if they are `void`. Parameters in block types should be named unless the block is initialized immediately.

If a block is large, i.e. more than 20 lines, it should move out-of-line into a local variable.

**For example:**

```objc
void (^blockName1)(void) = ^{
    // do some things
};
```

or

```objc
NSString * (^blockName2)(RIResponse *, NSDictionary *) = 
    ^ NSString * (RIResponse *response, NSDictionary *info) {
        // do some things
    };
```

**Not:**

```objc
void (^blockName1)(void) = ^ void (void) {
    // do some things
};
```

or

```objc
NSString * (^blockName2)(RIResponse *response, NSDictionary *info) = 
    ^ NSString * (RIResponse *response, NSDictionary *info) {
        // do some things
    };
```

## Protocols

In class declarations there should be a space between the type identifier and the name of the protocol encased in angle brackets. Multiple protocol identifiers should each be placed on a single line and angle brackets should each be placed on a single line too.
In method declarations there should not be a space between the type identifier and the protocol name encased in angle brackets. Multiple protocol identifiers should reside on the same line comma-separated.

**For example:**

```objc
@interface RIProtocoledClass : NSObject <NSCoding>

- (void)setDelegate:(id<RIAnyDelegate>)aDelegate;

@end
```

or 

```objc
@interface RIProtocoledClass : NSObject 
<
    NSCoding,
    NSURLProtocol,
    RISpecialProtocol
>

- (void)setDelegate:(id<RIAnyDelegate, RIAnotherDelegate>)aDelegate;

@end
```

**Not:**

```objc
@interface RIProtocoledClass : NSObject<NSCoding>

- (void)setDelegate:(id <RIAnyDelegate>)aDelegate;

@end
```

or 

```objc
@interface RIProtocoledClass : NSObject <NSCoding, NSURLProtocol, RISpecialProtocol>

- (void)setDelegate:(id <RIAnyDelegate,RIAnotherDelegate>)aDelegate;

@end
```

In protocol definitions, you may use `@optional` if not all the methods are required.

## Comments

When they are needed, comments should be used to explain **why** a particular piece of code does something. Any comments that are used must be kept up-to-date or deleted.

Block comments should generally be avoided, as code should be as self-documenting as possible, with only the need for intermittent, few-line explanations. This does not apply to those comments used to generate documentation.

## Literals

`NSString`, `NSDictionary`, `NSArray`, and `NSNumber` literals should be used whenever creating immutable instances of those objects. Pay special care that `nil` values not be passed into `NSArray` and `NSDictionary` literals, as this will cause a crash. `NSDictionary` literals, if not fit in a single line, should wrap according to their key-value pairs and vertically align with the colons.

**For example:**

```objc
NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul"];
NSDictionary *productManagers = @{@"iPhone" : @"Kate", @"iPad" : @"Kamal"};
NSDictionary *projectManagers = @{
    @"iPhone"       : @"Thomas", 
    @"iPad"         : @"Clarissa", 
    @"Mobile Web"   : @"Bill"
};
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingZIPCode = @10018;
```

**Not:**

```objc
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys:@"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingZIPCode = [NSNumber numberWithInteger:10018];
```

## CGRect Functions

When accessing the `x`, `y`, `width`, or `height` of a `CGRect`, always use the [`CGGeometry` functions](http://developer.apple.com/library/ios/#documentation/graphicsimaging/reference/CGGeometry/Reference/reference.html) instead of direct struct member access. From Apple's `CGGeometry` reference:

> All functions described in this reference that take CGRect data structures as inputs implicitly standardize those rectangles before calculating their results. For this reason, your applications should avoid directly reading and writing the data stored in the CGRect data structure. Instead, use the functions described here to manipulate rectangles and to retrieve their characteristics.

**For example:**

```objc
CGRect frame = self.view.frame;

CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
```

**Not:**

```objc
CGRect frame = self.view.frame;

CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
```

## Constants

Constants are preferred over in-line string literals or numbers, as they allow for easy reproduction of commonly used variables and can be quickly changed without the need for find and replace. Constants should be declared as `static` constants and not `#define`s unless explicitly being used as a macro.

**For example:**

```objc
static NSString * const RIAboutViewControllerCompanyName = @"The New York Times Company";

static const CGFloat RIImageThumbnailHeight = 50.0;
```

**Not:**

```objc
#define CompanyName @"Rocket Internet"

#define thumbnailHeight 2
```

## Enumerated Types

When using `enum`s, it is recommended to use the new fixed underlying type specification because it has stronger type checking and code completion. The SDK now includes a macro to facilitate and encourage use of fixed underlying types — `NS_ENUM()`

**Example:**

```objc
typedef NS_ENUM(NSInteger, RIAdRequestState) {
    RIAdRequestStateInactive,
    RIAdRequestStateLoading
};
```

## Case Statements

Braces are required for case statements.

**For example:**

```objc
switch (condition) {
    case 1: {
        // ...
        break;
    }
    case 2: {
        // ...
        // ...
        break;
    }
    default: { 
        // ...
        break;
    }
}
```

**Not:**

```objc
switch (condition) {
    case 1:
        // ...
        break;
    case 2: {
        // ...
        // Only multi-line example using braces
        break;
    }
    case 3:
        // ...
        break;
    default: 
        // ...
        break;
}
```

There are times when the same code can be used for multiple cases, and a fall-through should be used.  A fall-through is the removal of the `break` statement for a case thus allowing the flow of execution to pass to the next case value.  A fall-through should be commented for coding clarity.

```objc
switch (condition) {
    case 1:
        // ** fall-through! **
    case 2: {
        // code executed for values 1 and 2
        break;
    }
    default: {
        // ...
        break;
    }
}
```

When using an enumerated type for a switch, 'default' is not needed.   For example:

```objc
RILeftMenuTopItemType menuType = RILeftMenuTopItemMain;

switch (menuType) {
    case RILeftMenuTopItemMain: {
        // ...
        break;
    }
    case RILeftMenuTopItemShows: {
        // ...
        break;
    }
    case RILeftMenuTopItemSchedule: {
        // ...
        break;
    }
}
```

## Golden Path

When coding with conditionals, the left hand margin of the code should be the "golden" or "happy" path.  That is, don't nest `if` statements.  Multiple return statements are fine.

**For example:**

```objc
- (void)someMethod {
    if (![someOther boolValue]) {
        return;
    }
    //Do something important
}
```

**Not:**

```objc
- (void)someMethod {
    if ([someOther boolValue]) {
        //Do something important
    }
}
```

## Image Naming

Image names should be named consistently to preserve organization and developer sanity. They should be named as one camel case string with a description of their purpose, followed by the un-prefixed name of the class or property they are customizing (if there is one), followed by a further description of color and/or placement, and finally their state.

**For example:**

* `RefreshBarButtonItem` / `RefreshBarButtonItem@2x` and `RefreshBarButtonItemSelected` / `RefreshBarButtonItemSelected@2x`
* `ArticleNavigationBarWhite` / `ArticleNavigationBarWhite@2x` and `ArticleNavigationBarBlackSelected` / `ArticleNavigationBarBlackSelected@2x`.

Images that are used for a similar purpose should be grouped in respective groups in an Images folder.

## Booleans

Since `nil` resolves to `NO` it is unnecessary to compare it in conditions. Never compare something directly to `YES`, because `YES` is defined to 1 and a `BOOL` can be up to 8 bits.

This allows for more consistency across files and greater visual clarity.

**For example:**

```objc
if (!someObject) {
//Do something
}
```

**Not:**

```objc
if (someObject == nil) {
//Do something
}
```

-----

**For a `BOOL`, here are two examples:**

```objc
if (isAwesome)
if (![someObject boolValue])
```

**Not:**

```objc
if ([someObject boolValue] == NO)
if (isAwesome == YES) // Never do this.
```

-----

If the name of a `BOOL` property is expressed as an adjective, the property can omit the “is” prefix but specifies the conventional name for the get accessor, for example:

```objc
@property (assign, getter=isEditable) BOOL editable;
```
Text and example taken from the [Cocoa Naming Guidelines](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE).

## Singletons

Singleton objects should use a thread-safe pattern for creating their shared instance.
```objc
+ (instancetype)sharedInstance {
   static id sharedInstance = nil;

   static dispatch_once_t onceToken;
   dispatch_once(&onceToken, ^{
      sharedInstance = [[self alloc] init];
   });

   return sharedInstance;
}
```
This will prevent [possible and sometimes prolific crashes](http://cocoasamurai.blogspot.com/2011/04/singletons-your-doing-them-wrong.html).

# Other Objective-C Style Guides

If ours doesn't fit your tastes, have a look at some other style guides:

* [NYTimes](https://github.com/NYTimes/objective-c-style-guide)
* [Google](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml)
* [GitHub](https://github.com/github/objective-c-conventions)
* [Adium](https://trac.adium.im/wiki/CodingStyle)
* [Sam Soffes](https://gist.github.com/soffes/812796)
* [CocoaDevCentral](http://cocoadevcentral.com/articles/000082.php)
* [Luke Redpath](http://lukeredpath.co.uk/blog/my-objective-c-style-guide.html)
* [Marcus Zarra](http://www.cimgf.com/zds-code-style-guide/)
