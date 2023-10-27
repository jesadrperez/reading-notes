# The Art of Readable Code

Simple and Practical Techniques for Writing Better Code


## Part One: Surface-Level Improvements


### 1 Code Should be Easy to Understand

Code should be written to minimize the time it would take for someone else to understand it.


### 2 Packing Information into Names

The single theme for this chapter is: **pack more information into your names.** By this, we mean the reader can extract a lot of information just from reading the name.

Here are some specific tips we covered:

* **Use specific words**—for example, instead of `Get`, word like `Fetch` or `Download` might be better, depending on the context.
* **Avoid generic names** like `tmp` and `retval`, unless there's a specific reason to use them.
* **Use concrete names** that describe things in more detail—the name `ServerCanStart()` is vague compared to `CanListenOnPort()`.
* **Attach important details** to variable names—for example, append `_ms` to a variable whose value is in milliseconds or preprent `raw_` to an unprocessed variable that needs escaping.
* **Use longer names for larger scopes**—don't use cryptic one- or two-letter names for variables that span multiple screens; shorter names are better for variables that span only a few lines.
* **Use captilatization, underscores, and so on in a meaningful way**—for example, you can append "_" to class members to distinguish them from local variables.


### 3 Names that Cannot be Misconstrued

Thw best names are ones that can't be miconstrued—the person reading your code will understand it the way you meant it, and no other way. Unfortunately, a lot of English words are ambiguous when it comes to programming, such as `filter`, `length`, and `limit`.

Before you decide on a name, play devil's advocate and imagine how your name might be misunderstood. The best names are resistant to misinterpreatation.

When it comes to defininf an upper or lower limit for a value, `max_` and `min_` are good prefixes to use. For inclusive ranges, `first` and `last` are good. For inclusive/exclusive ranges, `begin` and `end` are best because they're the most idiomatic.

When naming a boolean, use words like `is` and `has` to make it clear that it's a boolean. Avoid negated terms (e.g. `disable_ssl`).

Beware of users' expectations about certain words. For example, users may expect `get()` or `size()` to be lightweight methods.


### 4 Aesthetics

Everyone prefers to read code that's aesthetically pleasing. By "formatting" your code in a consistent, meaningful way, you make it easier and faster to read.

Here are specific techniques we discussed:

* If multiple blocks of code are doing similar things, try to give them the same silhouette.
* Aligning parts of the code into "columns" can make code easy to skim through.
* If code mentions A, B, and C in one place, don't say B, C, and A in another. Pick a meaningful order and stick with it.
* Use empty lines to break apart large blocks into logical "paragraphs."

### 5 Knowing What to Comment

The purpose of a comment is to help the reader know what the writer knew when writing the code. This whole chapter is about realizing all the not-so-obvious nuggets of information you have about the code and writing those down.

What *not* to comment:

* Facts that can be quickly derived from the code itself.
* "Crutch comments" that make up for bad code (such as a bad function name)—fix the code instead.

Thoughts you should be recording include:

* Insights about why code is one way and not another ("directory commentary").
* Flaws in your code, by using markers like TODO: or XXX:.
* The "story" for how a constant got a value.

Put yourself in the reader's shoes:

* Anticipate which parts of your code will make readers say "Huh?" and comment those.
* Document any surprising behavior an average reader wouldn't expect.
* Use "big picture" comments at the file/class level to explain how all the pieces fit together.
* Summarize block of code with comments so that the reader doesn't get lost in the details.

### 6 Making Comments Precise and Compact

This chapter is about writing comments that pack as much information into as small a space as possible. Here are specific tips:

* Avoid pronouns like "it" and "this" when they can refer to multiple things.
* Describe a function's behavior with as much precision as is pracitical.
* Illustrate your comments with carefully chosen input/output examples.
* State the high-level intent of your code, rather than the obvious details.
* Use inline comments (e.g. `Function(/* arg = */ ...)`) to explain mysterious function arguments.
* Keep your comments brief by using words that pack a lot of meaning.

## Part Two: Simplifying Loops and Logic

### 7 Making Control Flow Easy to Read

There are a number of things you can do to make your code's control flow easier to read. 

When writing a comparison (`while (bytes_expected > bytes_received)`), it's better to put the changing value on the left side and the more stable value on the right (`while (bytes_received < bytes_expected)`). 

You can also reorder the blocks of an `if/else` statement. Generally, try to handle the positive/easier/interesting case first. Sometimes these criteria conflict, but when they don't, it's a good rule of thumb to follow.

Certain programming constructs, like the ternary operator (`: ?`), the `do/while` loop, and goto often results in unreadable code. It's usually best not to use them, as clearer alternatives almost always exist.

Nested code blocks require more concebtration to follow along. Each new nesting requires more context to be "pushed onto the stack" of the reader. Instead, opt for more "linear" code to avoid deep nesting. 

Returning early can remove nesting and clean up code in general. "Guard statements" (handling simple cases at the top of the function) are espically useful.

### 8 Breaking Down Giant Expressions

Giant expressions are heard to think about. This chapter showed a number of ways to break down them down so the reader can digest them piece by piece.

One simple technique is to introduce "explaining variables" that capture the value of some large subexpression. This approach has three benefits:

* It breaks down a giant expression into pieces.
* It documents the code by describing the subexpression with a succint name.
* It helps the reader identify the main "concepts" in the code.

Another technique is to manipulate your logic using De Morgan's laws—this technique can sometimes rewrite a boolean expression in a cleaner way (e.g., `if (!(a && !b))` turns into `(!a || b)`).

We showed an example where a complex logical condition was broken down into tiny statements like "`if (a < b) ...`. In fact, *all* of the improved-code examples in this chapter had `if` statements with no more than *two* values inside them. This setup is ideal. It may not always seem possible to do this—sometimes it requires "negating" the problem or considering the opposite of your goal.

Finally, even though this chapter is about breaking down individual expressions, these same techniques often apply to larger blocks of code, too. So be aggressive in breakdown complex logic where you set it.

### 9 Variables and Readability

This chapter is about how the variables in a program can quickly accumlate and become too much to keep track of. You can make your code easier to read by having fewer variables and making them as "lightweight" as possible. Specifically:

* **Elimate variables** that just get in the way. In particular, we showed a few examples of how to eliminate "intermediate results" varibales by handing the results immediately.
* **Reduce the scope of each variables** to be as small as possible. Move each variable to be a place where the fewest lines of code can see it. Out of sight is out of mind.
* **Prefer write-once variables.** Variables that are set only once (or `const`, `final`, or otherwise immutable) make code easier to understand.

## Part Three: Reorganizing Your Code

### 10 Extracting Unrelated Subproblems

A simple way to think about this chapter is to **separate the generic code from the project-specific code.** As it turns out, most code is generic. By building a large set of libraries and helper functions to solve the general problems, what's left will be a small core of what makes your program unique.

The main reason this technique helps is that it lets the programmer focus on smaller, well-defined problems that are detached from the rest of your projects. As a results, the solutions to those problems tend to be more thorough and correct. You might also be able to reuse them later.

### 11 One Task at a Time

This chapter illustrates a simple technique for organizing your code: **do only one task at a time.** 

If you have code that's difficult to read, try to list all of the tasks it's doing. Some of these tasks might easily become separate functions (or classes). Others might just become logical "paragraphs" within a single function. The exact details of how you separate these task isn't as important as the fact that they're separated. The hard part is accurately describing all the little things your program is doing.

### 12 Turning Thoughts into Code

This chapter discussed the simple technique of describing your program in plain English and using that description to help you write more natural code. This technique is deceptively simple, but very powerful. Looking at the words and phrases used in your description can help you identify which subproblems to break off.

But this process of "saying things in plain English" is applicable outside of just writing code. For example, one college computer lab policy states that when a student needs help debugging their program, they first have to explain the problem to a dedicated teddy bear in the corner of the room. Surprisingly, just describing the problem aloud can help the student fiqure out a solution. This technique is called "rubber ducking."

Another way to look at this: if you can't describe the problem or your design in words, something is probably missing or undefined. Getting a program (or any idea) into words can really force it into shape.

### 13 Writing Less Code

> Adventure, exictment—a Jedi craves not these things.  —Yoda

This chapter is about writing as little new code as possible. Each new line of code needs to be tested, documented, and maintained. Further, the more code in your codebase, the "heavier" it gets and harder it is to develop in.

You can avoid writing new lines of code by:

* Eliminating nonessential features from your product and not overengineering.
* Rethinking requirements to solve the easiest version og the problem that still gets the job done.
* Staying familiar with standard libraries by periodically reading through their entire APIs.

## Part Four: Selected Topics

### 14 Testing and Readability

