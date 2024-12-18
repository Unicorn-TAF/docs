---
title: Assertions
category: Testing
weight: 3
order: 4
---

Unicorn provides with built-in assertions mechanism to perform various types of checks in tests. These assertions are designed to be simple and easy to use, and they can be used to check the state of your application, the behavior of your code, and the correctness of your data.
Basically mechanism consists of two parts: **assertion methods** and so called **"matchers"**.

## Assertion methods

### Simple assertions
`Assert.IsTrue` - simple check if some boolean condition is true  
`Assert.IsFalse` - simple check if some boolean condition is false  
`Assert.Fail` - just syntax sugar for `throw new AssertionException`

### Assertions with matchers
More syntaxically understandable and self-describable assertion approach is combination of `Assert.That` method with a matcher. Matchers are sort of specific checks under objects. There are type specific matchers which perform check under object of specific type or it's inheritors and could not be applied to other types (there will be compile-time errors). Other group of matchers are type independent and designed to check general things, for example nullability or type of expected object. There is also a special matcher, which designed to negate action of any matcher, it allows to have just one implementation to perform both positive and negative assertion. In case of error `Assert.That` will throw an exception describing initial check and actual result. In case of **Steps** feature usage the mechanism allows to use just few built-in steps to report all checks in human readable form.
Here are some examples:

```csharp
using Unicorn.Taf.Core.Verification.Matchers;
using Unicorn.Taf.Core.Verification;

// check that object is null
Assert.That(objectUnderTest, Is.Null());

// check that object is NOT null using the same matcher with negation (Is.Not matcher)
Assert.That(objectUnderTest, Is.Not(Is.Null()));

// check that actual string is equal to some value
Assert.That(stringUnderTest, Is.EqualTo("some_excpected_value"));

// check that actual string is NOT equal to some value using the same matcher with negation (Is.Not matcher)
Assert.That(stringUnderTest, Is.Not(Is.EqualTo("some_excpected_value")));

// check that actual collection has scpecific items
Assert.That(collectionUnderCheck, Collection.HasItems(expectedItem));

// complex checks that EACH of collection items matches some specific matcher
Assert.That(integerArray, Collection.Each(Number.IsEven()));
Assert.That(stringsList, Collection.Each(Is.Not(Is.EqualTo("some_excpected_value"))));

// complex check that ANY of collection items is null
Assert.That(objectsList, Collection.Any(Is.Null()));

// check that actual string matches specific regular expression
Assert.That(stringUnderTest, Text.MatchesRegex("some-regex"));
```

Full list of build-in core matchers is available as children of:
 - `Unicorn.Taf.Core.Verification.Matchers.Collection`
 - `Unicorn.Taf.Core.Verification.Matchers.Is`
 - `Unicorn.Taf.Core.Verification.Matchers.Number`
 - `Unicorn.Taf.Core.Verification.Matchers.Text`

As result the mechanism is very convenient and saves many lines of code giving helpful functionality out of the box. Using this approach together with built-in **Steps** feature will result call `Do.Assertion.AssertThat(collection, Collection.Each(Text.MatchesRegex("value[0-9]")))` to very useful output for both logging and reporting (let's consider assertion fail case):

```
[Info]: STEP: Assert that collection of <String> Each element Matches regex 'value[0-9]'

Assertion failed.
Expected: Each element Matches regex 'value[0-9]'
But: element at index 1:was value_2
```

## Assertions chain
Assertions chain allows to perfrom multiple assertions silently and to get overall result after all assertion are called. If there are any failed assertions in chain the total assertion will fail listing all fails in the chain. This allows to gather more useful information in case of group of similar checks or sequence of checks under the same element.
Here is an example:

```csharp
new ChainAssert() // Initializing chain assert
    // performing any number of assertions
    .That(collection, Collection.HasItemsCount(3))
    .That(collection, Collection.Each(Number.IsEven()))
    .That(collection, Collection.Any(Is.EqualTo(2)))
    .AssertChain(); // perform total assertion (if method is not called, child assertion results will be ignored)
```

## Custom matchers

It's easy to create own matcher for any project specific check if none of built-in matchers suits. For this just need to inherit `TypeUnsafeMatcher` or `TypeSafeMatcher` with desired type (depending on your needs) and to implement `CheckDescription` property which basically describes nature of custom check and `Matches` method which will perform the check itself. Below is example of custom matcher to check if integer is odd:

```csharp
public class IsOddMatcher : TypeSafeMatcher<int>
{
    public override string CheckDescription => "is odd number";

    public override bool Matches(int actual)
    {
        // DescribeMismatch will help to generate useful error message describing actual state object under check
        DescribeMismatch(actual.ToString());
        return actual % 2 != 0;
    }
}
```