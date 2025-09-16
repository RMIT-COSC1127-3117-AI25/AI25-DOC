# FAQ about Prolog

These questions are about Prolog itself and its usage, not specific to a particular assessment type that uses Prolog.

## What is "vanilla" Prolog?

[Vanilla software](https://en.wikipedia.org/wiki/Vanilla_software) refers to software that have _not_ been customized or modified from their original form. Thus we mean "plain" Prolog, the original Prolog without advanced predicates or implementation specific ones.

The spirit of this restriction is to restrict yourself to predicates which are simply defining logical rules which are then used by Prolog's internal backwards chaining inference engine. This is as much as we want you to learn and the content which you are familiar with from tutorials (Prolog is more or less just doing resolution proofs automatically, like you do on paper!). Some predicates do 'extra-logical' work, that go beyond this basic inference, and those are the ones we want you to avoid.

In practice it is very hard for you to tell which predicates are 'vanilla' and which aren't without extraordinary research on your end, which is why we have given you lists of predicates which we specifically allow, and predicates which we specifically don't allow. After you work a bit with Prolog, you will most often know if a predicate is advanced or not.

Said so, if you want to use a predicate which is not mentioned in the spec in either direction, post it on the forums and we will let you know. Also look at next question.

## I want to get a head start and learn Prolog now! How can I do it?

Good on you for looking ahead, I have found these helpful in the past:

- An Introduction to Prolog (Appendix A). In: [An Introduction to Language Processing with Perl and Prolog](https://link.springer.com/chapter/10.1007/3-540-34336-9_16). Cognitive Technologies. 2006. Springer, Berlin, Heidelberg.
- Great overview video from Exercism.org: https://youtu.be/dKn-BbS_zQQ?si=fCAzrCbEV7G4ZW-Q
  - The actual Exercism exercises: https://exercism.org/tracks/prolog
- The Power of Prolog @ Metalevel: https://www.metalevel.at/prolog
- Four videos to get started in Prolog @ Classcentral: https://www.classcentral.com/classroom/youtube-prolog-programming-120169
- Prolog in one video @ Classcentral: https://www.classcentral.com/classroom/youtube-prolog-tutorial-106231

Of course, things will make more sense once you have seen predicate logic in class, as it is basically using predicate logic to program! 😉

## Is it ok to change the parameter structure of the predicates we are told to implement?
> eg:- `P(x)` could be changed to `P([H|T])`?

Yes, note this does not change the predicate signature itself, it is just changing the names used to refer to different parts of the argument.

## Is it ok to define multiple versions of the predicates (helpful for base and edge cases)?

Yes, and it would be almost impossible to complete the assignment without it.

## Is it ok to define helper functions?

Yes, these are called auxiliary predicates, and are in the spec already:

> You would probably need to use auxiliary predicates, and resort to recursive definitions.

## My solution prints false when the example doesn't, or warning: `Test tools_for_items_2: Test succeeded with choicepoint`
![Choice point example](img/prolog-choicepoint.png)

Both of these have the same root cause, and the short answer is that you don't need to worry about it.

The longer answer is: Fundamentally, the error is essentially saying that your predicate is finding only the one right answer, but in order to confirm that is the single correct answer it needs to backtrack through a bunch of choices it makes along the way, which is inefficient. However, a good solution will be written such that finding the single correct answer does not leave any unresolved choices, so it can return immediately.
From my perspective, it would be better if you could get rid of this, but in this course we will not penalise you for this behaviour. Usually we try to remove those dummy choice points, but sometimes it is not possible or it becomes a mess.

## Best way to check empty list in Prolog
> Are there any differences/considerations/trade-offs between \= [] and \+ length(someList, 0) ?

The first reads easier and more direct, so I wold use that.

In some circumstances you might even get different results. If someList is only partially instantiated; i.e. it is `[2,3,X]` where X is still a free variable that may be a continuation of the list, then length may be undefined, but at least 2. In this case the first is clear, `[2,3,X] \= []`, but I don't know whether Prolog would be able to tell that length>=2 and therefore the second would also fail.

And if you want to check if it is a list or not, just use is_list/1

## What do the question marks mean in the predicate: `tool_for_item(?Item, ?Tool)`?

These are the [argument mode indicators](https://www.swi-prolog.org/pldoc/man?section=argmode), `?` means it can be provided or not.

## I can't see the full output in Prolog, I am getting dots instead indicating it continues. How can I see everything?

Press `w` after it prints the dots to see the full output.

## OK, but can I make it to print all the answer always?

**Yes!**

The toplevel interface as well as the tracer will abbreviate long complex terms, so you will see things like this:

```prolog
?- blue_print(item7, A).
A = complex(item7, [base(item0), complex(item4, [base(item0), base(item1), base(item2), base(item3)]), complex(item5, [base(item0), base(item1), base(item2), base(...)])])
```

One way to interactively instruct SWI to show full terms is to press key `w` (write), and from now on SWI will print the full solution (even if it involves printing pages!). You can use `p` to return to the compact reporting.

To change the default behavior change flag `answer_write_options` via `set_prolog_flag/2`. For example to disable abbreviation fully and print arguments with spaces:

```prolog
?- set_prolog_flag(answer_write_options,
                   [ quoted(true),
                     portray(true),
                     spacing(next_argument)
                   ]).

?- current_prolog_flag(answer_write_options, X).
X = [quoted(true), portray(true), spacing(next_argument)].

?- blue_print(item7, A).
A = complex(item7, [base(item0), complex(item4, [base(item0), base(item1), base(item2), base(item3)]), complex(item5, [base(item0), base(item1), base(item2), base(item3)])]) ;
false.
```

For more details check documentation page [AllOutput](https://www.swi-prolog.org/FAQ/AllOutput.html).
