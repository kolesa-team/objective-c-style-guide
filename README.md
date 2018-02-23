# The official KolesaTeam Objective-C style guide.

This style guide outlines the coding conventions for "Колёса | Крыша | Маркет" company. It is a slightly modified and supplemented version of the official raywenderlich.com Objective-C style guide.

## Background

Here are some of the documents from Apple that informed the style guide. If something isn't mentioned here, it's probably covered in great detail in one of these:

* [The Objective-C Programming Language](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/Introduction/Introduction.html)
* [Coding Guidelines for Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
* [iOS App Programming Guide](http://developer.apple.com/library/ios/#documentation/iphone/conceptual/iphoneosprogrammingguide/Introduction/Introduction.html)

## Table of Contents

* [Language](#language)
* [Code Organization](#code-organization)
* [Spacing](#spacing)
* [Comments](#comments)
* [Naming](#naming)
  * [Underscores](#underscores)
  * [Tests](#tests)
* [Methods](#methods)
* [Variables](#variables)
* [Property Attributes](#property-attributes)
* [Nullability](#nullability)
* [Dot-Notation Syntax](#dot-notation-syntax)
* [Literals](#literals)
* [Constants](#constants)
* [Enumerated Types](#enumerated-types)
* [Case Statements](#case-statements)
* [Private Properties](#private-properties)
* [Property Shadowing](property-shadowing)
* [Booleans](#booleans)
* [Conditionals](#conditionals)
  * [Ternary Operator](#ternary-operator)
* [Init Methods](#init-methods)
* [Class Constructor Methods](#class-constructor-methods)
* [CGRect Functions](#cgrect-functions)
* [Golden Path](#golden-path)
* [Error handling](#error-handling)
* [Singletons](#singletons)
* [Line Breaks](#line-breaks)
* [Smiley Face](#smiley-face)
* [Xcode Project](#xcode-project)

## Language

US English should be used.

**Preferred:**
```objc
UIColor *myColor = UIColor.whiteColor;
```

**Not Preferred:**
```objc
UIColor *myColour = UIColor.whiteColor;
```

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

## Spacing

* Indent using 4 spaces (This is a default in Xcode. You can verify it in `Preferences → Text Editing → Indentation`). Never indent with tabs.
* Better not to have lines exceeding 100 characters. The option `Page guide at column: 100` from `Preferences → Text Editing → Editing` can help in this.
* Method braces and other braces (`if`/`else`/`switch`/`while` etc.) always open on the same line as the statement but close on a new line.
* Any piece of documentation or comment should be preceeded with a blank line. Except the cases when such a block immediately follows an opening curly brace.

**Preferred:**
```objc
if (user.isHappy) {
    // Do something
} else {
    // Do something else
}
```

**Not Preferred:**
```objc
if (user.isHappy)
{
    // Do something
}
else {
    // Do something else
}
```

* There should be exactly one blank line between methods to aid in visual clarity and organization. Whitespace within methods should separate functionality, but often there should probably be new methods.
* Prefer using auto-synthesis. But if necessary, `@synthesize` and `@dynamic` should each be declared on new lines in the implementation.
* Colon-aligning method invocation should often be avoided. There are cases where a method signature may have >= 3 colons and colon-aligning makes the code more readable. Please do **NOT** however colon align methods containing blocks because Xcode's indenting makes it illegible.

**Preferred:**
```objc
// blocks are easily readable
[UIView animateWithDuration:1.0 animations:^{
    // something
} completion:^(BOOL finished) {
    // something
}];
```

**Not Preferred:**
```objc
// colon-aligning makes the block indentation hard to read
[UIView animateWithDuration:1.0
                 animations:^{
                     // something
                 }
                 completion:^(BOOL finished) {
                     // something
                 }];
```

* To boost readability, control flow blocks should be followed by a blank line. It is optional to have an empty line before the block.

**Preferred:**
```objc
// something
if (condition) {
    // Do something
}

// something
```

**Not Preferred**
```objc
// something
if (condition) {
    // Do something
}
// something
```

* Unless `return` is the only statement in the block it should always be preceded by a blank line.

**Preferred:**
```objc
­- (BOOL)isTrue {
    return YES;
}

­- (BOOL)isComplicated {
    BOOL retVal = NO;
    
    return retVal;
}
```

**Not Preferred:**
```objc
- (BOOL)isTrue {

    return YES;
}

­- (BOOL)isComplicated {
    BOOL retVal = NO;
    return retVal;
}
```

* In case of wrapping a method with blocks, the statements in the blocks should be indented by additional 4 spaces.

**Preferred:**
```objc
[[KKSomeClass sharedInstance] someLongMethod:KKSomeMethod
    parameters:parameters
    success:^(id obj) {
        // something
    } failure:^(id obj) {
        // something
    }];
```

**Not Preferred:**
```objc
[[KKSomeClass sharedInstance] someLongMethod:KKSomeMethod
    parameters:parameters
    success:^(id obj) {
    // something
    } failure:^(id obj) {
    // something
    }];
```

* When wrapping a method signature, which cannot be colon-aligned, all lines starting with the second one should be indented by 8 spaces.

**Preferred:**
```objc
­- (void)application:(UIApplication *)application
        performActionForShortcutItem:(UIApplicationShortcutItem *)shortcutItem
        completionHandler:(void (^)(BOOL))completionHandler {
    // something
}
```

**Not Preferred:**
```objc
­- (void)application:(UIApplication *)application
    performActionForShortcutItem:(UIApplicationShortcutItem *)shortcutItem
    completionHandler:(void (^)(BOOL))completionHandler {
    // something
}
```

**Not Preferred:**
```objc
­- (void)application:(UIApplication *)application
performActionForShortcutItem:(UIApplicationShortcutItem *)shortcutItem
completionHandler:(void (^)(BOOL))completionHandler {
    // something
}
```

* There should always be a blank line after `@interface`/`@implementation` and before `@end`.

**Preferred:**
```objc
@interface KKSomeClass : NSObject <KKSomeProtocol>

// something

@end
```

**Preferred:**
```objc
@implementation KKSomeClass

@synthesize someProperty = _someProperty;
// something

@end
```

**Not Preferred:**
```objc
@interface KKSomeClass : NSObject <KKSomeProtocol>
// something
@end
```

**Not Preferred:**
```objc
@implementation KKSomeClass

@synthesize someProperty = _someProperty;
// something
@end
```

* When declaring an `id` property or variable there should **NOT** be a space between `id` and `<`.

**Preferred:**
```objc
@property (strong, nonatomic, nonnull) id<SomeProtocol> someProperty;
```

**Not Preferred:**
```objc
@property (strong, nonatomic, nonnull) id <SomeProtocol> someProperty;
```

## Comments

When they are needed, comments should be used to explain **why** a particular piece of code does something. Any comments that are used must be kept up-to-date or deleted.

Block comments should generally be avoided, as code should be as self-documenting as possible, with only the need for intermittent, few-line explanations. *Exception: This does not apply to those comments used to generate documentation.*

## Naming

Apple naming conventions should be adhered to wherever possible, especially those related to [memory management rules](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html) ([NARC](http://stackoverflow.com/a/2865194/340508)).

Long, descriptive method and variable names are good.

**Preferred:**
```objc
UIButton *settingsButton;
```

**Not Preferred:**
```objc
UIButton *setBut;
```

A three or two letter prefix should always be used for class names and constants, however may be omitted for Core Data entity names.

Constants should be camel-case with all words capitalized and prefixed by the related class name for clarity.

**Preferred:**
```objc
static NSTimeInterval const RWTTutorialViewControllerNavigationFadeAnimationDuration = 0.3;
```

**Not Preferred:**
```objc
static NSTimeInterval const fadetime = 1.7;
```

Properties should be camel-case with the leading word being lowercase. Use auto-synthesis for properties rather than manual @synthesize statements unless you have good reason.

**Preferred:**
```objc
@property (strong, nonatomic) NSString *descriptiveVariableName;
```

**Not Preferred:**
```objc
id varnm;
```

### Underscores

When using properties, instance variables should always be accessed and mutated using `self.`. This means that all properties will be visually distinct, as they will all be prefaced with `self.`. 

An exception to this: inside initializers, the backing instance variable (i.e. `_variableName`) should be used directly to avoid any potential side effects of the getters/setters.

Local variables should not contain underscores.

### Tests

The following naming convention for tests helps better understand the behavior:

```objc
- (void)test<Method|Action|Whatever>_<success|failure>_<expected result="">
```

**For Example:**
```objc
- (void)testAuthorize_success_isTrue {
    // test statements
}

- (void)testDateRange_success_isWithinThreeDays {
    // test statements
}

- (void)testFoo_failure_isBar {
    // test statements
}
```

## Methods

In method signatures, there should be a space after the method type (-/+ symbol). There should be a space between the method segments (matching Apple's style). Always include a keyword and be descriptive with the word before the argument which describes the argument.

The usage of the word "and" is reserved. It should not be used for multiple parameters as illustrated in the `initWithWidth:height:` example below.

**Preferred:**
```objc
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
- (void)sendAction:(SEL)aSelector to:(id)anObject forAllCells:(BOOL)flag;
- (id)viewWithTag:(NSInteger)tag;
- (instancetype)initWithWidth:(CGFloat)width height:(CGFloat)height;
```

**Not Preferred:**
```objc
-(void)setT:(NSString *)text i:(UIImage *)image;
- (void)sendAction:(SEL)aSelector :(id)anObject :(BOOL)flag;
- (id)taggedView:(NSInteger)tag;
- (instancetype)initWithWidth:(CGFloat)width andHeight:(CGFloat)height;
- (instancetype)initWith:(int)width and:(int)height; // Never do this.
```

## Variables

Variables should be named as descriptively as possible. Single letter variable names should be avoided except in `for()` loops.

Asterisks indicating pointers belong with the variable, e.g., `NSString *text` not `NSString* text` or `NSString * text`, except in the case of constants.

[Private properties](#private-properties) should be used in place of instance variables whenever possible. Although using instance variables is a valid way of doing things, by agreeing to prefer properties our code will be more consistent. 

Direct access to instance variables that 'back' properties should be avoided except in initializer methods (`init`, `initWithCoder:`, etc…), `dealloc` methods and within custom setters and getters. For more information on using Accessor Methods in Initializer Methods and dealloc, see [here](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6).

**Preferred:**
```objc
@interface RWTTutorial : NSObject

@property (strong, nonatomic) NSString *tutorialName;

@end
```

**Not Preferred:**
```objc
@interface RWTTutorial : NSObject {
    NSString *tutorialName;
}
```

## Property Attributes

Property attributes should be explicitly listed, and will help new programmers when reading the code. The order of properties should be: `storage` (weak, strong, assign, copy), `atomicity` (atomic, nonatomic), `nullability` (nullable, nonnull), `access` (readonly, readwrite), and `getter/setter`.

**Preferred:**
```objc
@property (weak, nonatomic) IBOutlet UIView *containerView;
@property (strong, nonatomic) SomeClass *someClassObject;
@property (copy, nonatomic, nonnull, readonly) NSArray *someArray;
```

**Not Preferred:**
```objc
@property (nonatomic, weak) IBOutlet UIView *containerView;
@property (nonatomic) SomeClass *someClassObject;
@property (readonly, nonatomic, copy, nonnull) NSArray *someArray;
```

Properties with mutable counterparts (e.g. `NSString`) should prefer `copy` instead of `strong`. 
Why? Even if you declared a property as `NSString` somebody might pass in an instance of an `NSMutableString` and then change it without you noticing that.

**Preferred:**
```objc
@property (copy, nonatomic) NSString *tutorialName;
```

**Not Preferred:**
```objc
@property (strong, nonatomic) NSString *tutorialName;
```

## Nullability

When creating a new class, one should always explicitly specify nullability of its properties, method parameters and return values. For new properties in legacy code nullability should be omitted as it will trigger a compiler warning saying that nullability is not specified everywhere.

It is important to understand, though, that assigning `nil` to a `nonnull` property **won't** cause a crash. Compiler as well, **won't** warn if a `nullable` value is assigned to a `nonnull` property. Specifying nullability is a convention to make code more legible and to provide for compatibility of Objective-C and Swift code.

* If a property is `nonnull`, one guarantees that property will never be a `nil`. Otherwise, if it is `nullable`, we say that sometimes property can turn out to be a `nil`.
* As for return values, `nonnull` makes sure that the method **won't** return a `nil`; `nullable` return value states that in some cases `nil` can be returned by the method.
* A `nullable` method parameter highlights that a passed value can turn out to be a `nil`, even if it is not explicitly `nullable` (for instance, `NSDictionary` values accessed by keys); Likewise, one specifies `nonnull` when a passed value might **never** be a `nil`.

## Dot-Notation Syntax

Dot syntax is purely a convenient wrapper around accessor method calls. When you use dot syntax, the property is still accessed or changed using getter and setter methods. Read more [here](https://developer.apple.com/library/ios/documentation/cocoa/conceptual/ProgrammingWithObjectiveC/EncapsulatingData/EncapsulatingData.html)

Dot-notation should **always** be used for accessing and mutating properties, as it makes code more concise. Bracket notation is preferred in all other instances.

**Preferred:**
```objc
NSInteger arrayCount = self.array.count;
view.backgroundColor = UIColor.orangeColor;
UIApplication.sharedApplication.delegate;
```

**Not Preferred:**
```objc
NSInteger arrayCount = [self.array count];
[view setBackgroundColor:[UIColor orangeColor]];
[UIApplication sharedApplication].delegate;
```

## Literals

`NSString`, `NSDictionary`, `NSArray`, and `NSNumber` literals should be used whenever creating immutable instances of those objects. Pay special care that `nil` values can not be passed into `NSArray` and `NSDictionary` literals, as this will cause a crash.

**Preferred:**
```objc
NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul"];
NSDictionary *productManagers = @{@"iPhone": @"Kate", @"iPad": @"Kamal", @"Mobile Web": @"Bill"};
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingStreetNumber = @10018;
```

**Not Preferred:**
```objc
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingStreetNumber = [NSNumber numberWithInteger:10018];
```

## Constants

Constants are preferred over in-line string literals or numbers, as they allow for easy reproduction of commonly used variables and can be quickly changed without the need for find and replace. Constants should be declared as `static` constants and not `#define`s unless explicitly being used as a macro.

**Preferred:**
```objc
static NSString * const RWTAboutViewControllerCompanyName = @"RayWenderlich.com";
static CGFloat const RWTImageThumbnailHeight = 50.0;
```

**Not Preferred:**
```objc
#define CompanyName @"RayWenderlich.com"
#define thumbnailHeight 2
```

## Enumerated Types

When using `enum`s, it is recommended to use the new fixed underlying type specification because it has stronger type checking and code completion. The SDK now includes a macro to facilitate and encourage use of fixed underlying types: `NS_ENUM()`

**For Example:**
```objc
typedef NS_ENUM(NSInteger, RWTLeftMenuTopItemType) {
    RWTLeftMenuTopItemMain,
    RWTLeftMenuTopItemShows,
    RWTLeftMenuTopItemSchedule
};
```

You can also make explicit value assignments (showing older k-style constant definition):

```objc
typedef NS_ENUM(NSInteger, RWTGlobalConstants) {
    RWTPinSizeMin = 1,
    RWTPinSizeMax = 5,
    RWTPinCountMin = 100,
    RWTPinCountMax = 500,
};
```

Older k-style constant definitions should be **avoided** unless writing CoreFoundation C code (unlikely).

**Not Preferred:**
```objc
enum GlobalConstants {
    kMaxPinSize = 5,
    kMaxPinCount = 500,
};
```

In `enum`s 'undefined'/'missing' case should be the first one listed to make it evaluate to `0`. If there is no such a case, then the first option should start from `1`.

**Preferred:**
```objc
typedef NS_ENUM(NSInteger, FooType) {
    FooTypeUndefined,
    FooTypeFirst,
    FooTypeSecond
};
```

**Not Preferred:**
```objc
typedef NS_ENUM(NSInteger, FooType) {
    FooTypeFirst,
    FooTypeSecond,
    FooTypeUndefined
};
```

**Preferred:**
```objc
typedef NS_ENUM(NSInteger, FooType) {
    FooTypeFirst = 1,
    FooTypeSecond
};
```

**Not Preferred:**
```objc
typedef NS_ENUM(NSInteger, FooType) {
    FooTypeFirst,
    FooTypeSecond
};
```

## Case Statements

Braces are not required for case statements, unless enforced by the complier. When a case contains more than one line, braces should be added.

```objc
switch (condition) {
    case 1:
        // ...
        break;
    case 2: {
        // ...
        // Multi-line example using braces
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

There are times when the same code can be used for multiple cases, and a fall-through should be used. A fall-through is the removal of the 'break' statement for a case thus allowing the flow of execution to pass to the next case value. A fall-through should be commented for coding clarity.

```objc
switch (condition) {
    case 1:
        // ** fall-through! **
    case 2:
        // code executed for values 1 and 2
        break;
    default: 
        // ...
        break;
}
```

When using an enumerated type for a switch, 'default' is not needed.

**For Example:**
```objc
RWTLeftMenuTopItemType menuType = RWTLeftMenuTopItemMain;

switch (menuType) {
    case RWTLeftMenuTopItemMain:
        // ...
        break;
    case RWTLeftMenuTopItemShows:
        // ...
        break;
    case RWTLeftMenuTopItemSchedule:
        // ...
        break;
}
```

## Private Properties

Private properties should be declared in class extensions (anonymous categories) in the implementation file of a class. Named categories (such as `RWTPrivate` or `private`) should never be used unless extending another class. The Anonymous category can be shared/exposed for testing using the <headerfile>+Private.h file naming convention.

**For Example:**
```objc
@interface RWTDetailViewController ()

@property (strong, nonatomic) GADBannerView *googleAdView;
@property (strong, nonatomic) ADBannerView *iAdView;
@property (strong, nonatomic) UIWebView *adXWebView;

@end
```

## Property Shadowing

If it is necessary to make a property `readonly` for other classes and `readwrite` inside the class, one shall to declare the property two times: as `readonly` in `.h` file and as `readwrite` in the class extension in `.m` file. All the other attributes of the property must be repeated.

**For Example:**
```objc
// In .h file
@interface MyClass: NSObject

@property (assign, nonatomic, readonly) NSUInteger viewsCount;

@end


// In .m file
@interface MyClass ()

@property (assign, nonatomic, readwrite) NSUInteger viewsCount;

@end
```

In situations when it is solely needed to initialize a property and never mutate it later, one should not shadow properties. In initializers we directly work with instance variables by means of underscore - `_viewsCount`, and this enables us to ignore `readonly` attribute.

## Booleans

Objective-C uses `YES` and `NO`. Therefore `true` and `false` should only be used for CoreFoundation, C or C++ code. Since `nil` resolves to `NO` it is unnecessary to compare it in conditions. Never compare something directly to `YES`, because `YES` is defined to 1 and a `BOOL` can be up to 8 bits.

This allows for more consistency across files and greater visual clarity.

**Preferred:**
```objc
if (someObject) {}
if (![anotherObject boolValue]) {}
```

**Not Preferred:**
```objc
if (someObject == nil) {}
if ([anotherObject boolValue] == NO) {}
if (isAwesome == YES) {} // Never do this.
if (isAwesome == true) {} // Never do this.
```

If the name of a `BOOL` property is expressed as an adjective, the property can omit the “is” prefix but specifies the conventional name for the get accessor, for example:

```objc
@property (assign, getter=isEditable) BOOL editable;
```
Text and example taken from the [Cocoa Naming Guidelines](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE).

## Conditionals

Conditional bodies should always use braces even when a conditional body could be written without braces (e.g., it is one line only) to prevent errors. These errors include adding a second line and expecting it to be part of the if-statement. Another, [even more dangerous defect](http://programmers.stackexchange.com/a/16530) may happen where the line "inside" the if-statement is commented out, and the next line unwittingly becomes part of the if-statement. In addition, this style is more consistent with all other conditionals, and therefore more easily scannable.

**Preferred:**
```objc
if (!error) {
    return success;
}
```

**Not Preferred:**
```objc
if (!error)
    return success;
```

**Not Preferred:**
```objc
if (!error) return success;
```

### Ternary Operator

The Ternary operator, `?:` , should only be used when it increases clarity or code neatness. A single condition is usually all that should be evaluated. Evaluating multiple conditions is usually more understandable as an `if` statement, or refactored into instance variables. In general, the best use of the ternary operator is during assignment of a variable and deciding which value to use.

Non-boolean variables should be compared against something, and parentheses are added for improved readability. If the variable being compared is a boolean type, then no parentheses are needed.

**Preferred:**
```objc
NSInteger value = 5;
result = (value != 0) ? x : y;

BOOL isHorizontal = YES;
result = isHorizontal ? x : y;
```

**Not Preferred:**
```objc
result = a > b ? x = c > d ? c : d : y;
```

## Init Methods

Init methods should follow the convention provided by Apple's generated code template. A return type of 'instancetype' should also be used instead of 'id'.
```objc
- (instancetype)init {
    self = [super init];
    if (self) {
        // ...
    }

    return self;
}
```

## Class Constructor Methods

Where class constructor methods are used, these should always return type of 'instancetype' and never 'id'. This ensures the compiler correctly infers the result type. 

```objc
@interface Airplane

+ (instancetype)airplaneWithType:(RWTAirplaneType)type;

@end
```

More information on instancetype can be found on [NSHipster.com](http://nshipster.com/instancetype/).

## CGRect Functions

When accessing the `x`, `y`, `width`, or `height` of a `CGRect`, always use the [`CGGeometry` functions](https://developer.apple.com/documentation/coregraphics/cggeometry) instead of direct struct member access. From Apple's `CGGeometry` reference:

> All functions described in this reference that take CGRect data structures as inputs implicitly standardize those rectangles before calculating their results. For this reason, your applications should avoid directly reading and writing the data stored in the CGRect data structure. Instead, use the functions described here to manipulate rectangles and to retrieve their characteristics.

**Preferred:**
```objc
CGRect frame = self.view.frame;

CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
CGRect frame = CGRectMake(0.0, 0.0, width, height);
```

**Not Preferred:**
```objc
CGRect frame = self.view.frame;

CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
CGRect frame = (CGRect){ .origin = CGPointZero, .size = frame.size };
```

## Golden Path

When coding with conditionals, the left hand margin of the code should be the "golden" or "happy" path. That is, don't nest `if` statements. Multiple return statements are OK.

**Preferred:**
```objc
- (void)someMethod {
    if (![someOther boolValue]) {
	    return;
    }

    // Do something important
}
```

**Not Preferred:**
```objc
- (void)someMethod {
    if ([someOther boolValue]) {
        // Do something important
    }
}
```

## Error handling

When methods return an error parameter by reference, switch on the returned value, not the error variable.

**Preferred:**
```objc
NSError *error;
if (![self trySomethingWithError:&error]) {
    // Handle error
}
```

**Not Preferred:**
```objc
NSError *error;
[self trySomethingWithError:&error];
if (error) {
    // Handle error
}
```

Some of Apple’s APIs write garbage values to the error parameter (if non-NULL) in successful cases, so switching on the error can cause false negatives (and subsequently crash).

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

## Line Breaks

**For Example:**
```objc
self.productsRequest = [[SKProductsRequest alloc] initWithProductIdentifiers:productIdentifiers];
```

A long line of code like this should be carried on to the second line adhering to this style guide's Spacing section.

```objc
self.productsRequest = [[SKProductsRequest alloc] 
    initWithProductIdentifiers:productIdentifiers];
```

## Smiley Face

It is very important to have the correct smile signifying the immense amount of happiness and excitement for the coding topic. The end square bracket is used because it represents the largest smile able to be captured using ascii art. A half-hearted smile is represented if an end parenthesis is used, and thus not preferred.

**Preferred:**
```objc
:]
```

**Not Preferred:**
```objc
:)
```

## Xcode project

The physical files should be kept in sync with the Xcode project files in order to avoid file sprawl. Any Xcode groups created should be reflected by folders in the filesystem. Code should be grouped not only by type, but also by feature for greater clarity.

When possible, always turn on "Treat Warnings as Errors" in the target's Build Settings and enable as many [additional warnings](http://boredzo.org/blog/archives/2009-11-07/warnings) as possible. If you need to ignore a specific warning, use [Clang's pragma feature](http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas).

# Other Objective-C Style Guides

If ours doesn't fit your tastes, have a look at some other style guides:

* [Google](http://google.github.io/styleguide/objcguide.html)
* [Robots & Pencils](https://github.com/RobotsAndPencils/objective-c-style-guide)
* [New York Times](https://github.com/NYTimes/objective-c-style-guide)
* [Sam Soffes](https://gist.github.com/soffes/812796)
* [CocoaDevCentral](http://cocoadevcentral.com/articles/000082.php)
* [Marcus Zarra](http://www.cimgf.com/zds-code-style-guide/)
