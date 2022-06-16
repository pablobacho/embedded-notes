# C Firmware Style Guide

By Pablo Bacho

Updated on Mar 20th, 2021.

This work is licensed under a [Creative Commons Attribution-ShareAlike 4.0 International](http://creativecommons.org/licenses/by-sa/4.0/) License.

![CC-BY-SA 4.0 International](https://licensebuttons.net/l/by-sa/4.0/88x31.png)

- [Introduction](#introduction)
- [Project structure](#project-structure)
  - [Creating folders and files](#creating-folders-and-files)
  - [`/project-name`](#project-name)
  - [`/project-name/src`](#project-namesrc)
  - [`/project-name/include`](#project-nameinclude)
  - [`/project-name/libraries`](#project-namelibraries)
  - [`/project-name/resources`](#project-nameresources)
  - [`/project-name/test`](#project-nametest)
  - [`/project-name/docs`](#project-namedocs)
  - [`/project-name/build`](#project-namebuild)
  - [`/project-name/releases`](#project-namereleases)
  - [`/project-name/tools`](#project-nametools)
  - [`/project-name/templates`](#project-nametemplates)
- [General styling](#general-styling)
  - [Naming convention](#naming-convention)
  - [Hierarchy](#hierarchy)
  - [Acronyms](#acronyms)
  - [Indentation](#indentation)
  - [Braces](#braces)
  - [Line width](#line-width)
- [Variables](#variables)
  - [Declaration](#declaration)
  - [Fixed-width data types](#fixed-width-data-types)
  - [Variable scope](#variable-scope)
  - [`const` keyword](#const-keyword)
  - [`static` keyword](#static-keyword)
  - [`volatile` keyword](#volatile-keyword)
- [Operators](#operators)
  - [Styling](#styling)
  - [Operator precedence](#operator-precedence)
  - [Bit-wise operators](#bit-wise-operators)
- [Numeric literals](#numeric-literals)
- [Comments](#comments)
  - [Comment delimiters](#comment-delimiters)
  - [Grammar is important](#grammar-is-important)
  - [Documentation](#documentation)
  - [Comment headers](#comment-headers)
  - [`TODO` and `FIXME` comments](#todo-and-fixme-comments)
  - [Disabling code using comments](#disabling-code-using-comments)
- [Functions](#functions)
  - [Styling](#styling-1)
  - [Length](#length)
  - [Return codes](#return-codes)
  - [Interrupt Service Routines (ISR)](#interrupt-service-routines-isr)
- [Conditionals](#conditionals)
  - [Styling](#styling-2)
  - [Use of braces](#use-of-braces)
  - [Performing actions in conditions](#performing-actions-in-conditions)
- [Other](#other)
  - [Endianness](#endianness)
  - [3rd-party code](#3rd-party-code)



## Introduction

This document covers my firmware development style in C for embedded systems. These guidelines make my project structure and code style consistent across different projects.

The goal of these guidelines is reducing the chance of bugs, with a strong focus on making your code more readable. If you write readable, clean and consistent code, you are less likely to write buggy code.

There are three reasons why I am writing and sharing this document:

1. It makes it easy for others to understand the reasoning behind your work. Specially practical when sharing development within a team, or in the event of a new developer joining or taking over.
2. Being consistent across projects saves you from spending time thinking how to organize and do things, enabling you work more efficiently. Having your method written down keeps you from making mistakes.
3. I have been teaching embedded programming for several years, teaching my students to follow these guidelines. Writing them down makes it easier to share them.

If you would like to follow this style guide, please know that this is not meant to be a standard for firmware development. Common sense and reasoning should be used applying these guidelines to your own projects. This guide does not cover every eventuality and exceptions can be made when reasoned properly. I encourage you to fork this guide and make it fit your needs and personal style.

As a general rule of thumb, if a toolchain's best practice goes against this style guide in any way, go with the toolchain's recommendation. I think a project becomes more readable when best practices of the toolchain are followed and everything is the way a developer familiar with the toolchain expects it to be.

**Readability is king. When making exceptions always do them for the sake of readability.**



## Project structure

### Creating folders and files

I like using complete words for names. I try to avoid abbreviations when possible unless they are standard practice, improve readability, or there is a technical limit. Do not use names so long they do not fit in the project tree view of your development environment, though.

Always use all-lowercase folder and file names. No spaces or special characters other than hyphens (`-`) and underscores (`_`). This is great for portability between platforms. My personal preference is using hyphens (`-`).

Use hierarchy to be specific. Start always with the broadest item and narrow it down. This applies to folder structure and filenames.

Only necessary folders shall be created. In other words, do not create empty folders without reason. Feel free to add sub-folders when needed for clarity.

All folders and files are committed to the version control system unless otherwise stated.

When in conflict, always use your toolchain default folder structure. Using your toolchain defaults will make your project easily understood by developers acquainted with the toolchain, and will save you from dumb errors due to non-default paths. This organization scheme is a suggestion for when no other methods are implemented.

### `/project-name`

This folder contains at least the following files:
* README.md
* COPYRIGHT
* Project files, Makefiles or similar
* Version control system files (such as *.gitignore*)

### `/project-name/src`

Contains C source files (`.c`).

Executable projects **always** have a `main.c` file in this folder indicating point of entry. Library projects do not have a `main.c` file.

In projects with many source files, I might group them using sub-folders to make it easy to find them (e.g. `/project-name/src/display`).

I know earlier I said "avoid abbreviations", but `src` is so common practice, that it can be said that most toolchains and developers will expect a folder with this name, and certainly they will know its purpose.

### `/project-name/include`

Contains header files (`.h`). Add this folder to the *include path* of the toolchain. Many toolchains will use `inc` by default instead. As advised, follow your toolchain defaults.

### `/project-name/libraries`

This folder contains dependencies in source code form and/or pre-compiled libraries. Each dependency shall be in its own folder.

When using an external library repository, make a copy of the libraries required by the project in this folder. Having a snapshot of your dependencies minimizes the number of bugs introduced by unintended library changes, gives you full control when upgrading your project and makes it more portable.

Some toolchains have other preferred names for the dependency folder, such as `lib` or `components`. In that case, use the toolchain's defaults.

### `/project-name/resources`

Contains non-C resources such as images, certificates, fonts, and other files to be embedded into the output binary or flashed alongside it. If more than one type of resource is used, put each one in a different folder (`images` for images, `certificates` for certificates, etc.).

When storing sensitive information such as certificates, passwords, etc., consider excluding them from version control.

### `/project-name/test`

Contains test files. This includes any file used for testing and verification, whether it is code running on the microcontroller or any program running on a computer for stimulus generation and verification.

### `/project-name/docs`

Documentation files. When using an automated tool its temporary output should be excluded from version control. Generated documentation is always included in version control.

Other than code documentation, this is also the folder to store documents such as datasheets, relevant application notes, etc. Organize your files accordingly.

### `/project-name/build`

Contains temporary build files generated by the compiler.

If removed, the project is not affected. This folder should be excluded from version control.

### `/project-name/releases`

If compiled binaries need to be saved, this is the folder to do it. Only images ready to be flashed shall be stored in this folder. Supporting files such as checksums or changelogs are acceptable, but not files that need to be processed before flashing.

### `/project-name/tools`

Scripts and other tools. Normally I will store here helper scripts such as flash image builders, typeface to C converters, etc. If preserving the whole toolchain is necessary for exact replication of binaries in the future, this is the folder to use for it.

### `/project-name/templates`

Sometimes I find useful having templates. Common templates are for `.c` and `.h` files, including the comment header, include guards, etc. Snippets can also be stored here. Some toolchains use other files for defining storage partitioning, memory contents, etc. Keeping templates of those files easily accessible saves time.



## General styling

### Naming convention

When naming variables, functions, etc., be explicit. Use long names that clearly state the purpose of the item.

Do:

```
uint16_t number_of_pages;
```

Don't:

```
uint16_t n_pags;
```

Commonly used variable names such as `i` and `j`. are accepted in the context of loops. If these variables are used outside loops (other than **right before** for initialization or **right after**) use long meaningful names instead.

Separate words with underscores (`_`). Do not use *camelCase* or *PascalCase*.

Do:

```
void do_something_awesome(uint8_t candy_corn);
uint16_t highest_score;
```

Don't:

```
void doSomethingAwesome(uint8_t candyCorn);
uint16_t highestScore;
```

Constants and `#define` macro names are all-uppercase, using underscores (`_`) as word separator.

Do:

```
const double DISPLAY_GAMMA = 1.22;
#define RELAY_PIN 5
```

Custom data types are spelled all-lowercase, with a trailing `_t` at the end of their name.

Do:

```
typedef uint32_t timer_t;
timer_t my_little_timer;
```

### Hierarchy

Name resources in a top-bottom hierarchy, starting with the broader entity.

Do:

```
uint8_t network_dns_server_1[4]; // Network > DNS > Server 1
```

Don't:

```
uint8_t server1_dns[4]; // No hierarchy
```

### Acronyms

Beware acronyms and abbreviations. Well-known acronyms in embedded systems such as `gpio`, `uart`, `led`, etc. are acceptable. When using an acronym or abbreviation it is best to include its meaning in a comment where the resource is declared.

### Indentation

Prefer spaces over tabs. Indentation size is 4 spaces.

An example of breaking this rule to follow the toolchain defaults is when programming Arduino, where default indentation size is 2 spaces.

### Braces

Braces shall always surround `if`, `else`, `while`... blocks of code. This includes single statements, but they are not necessary for empty statements.

Do:

```
if(my_variable != 0) {
    return 1; // Single statement
}

while(1); // Empty statement
```

Don't:
```
if(my_variable != 0) return 1;
```

### Line width

There is a never-ending discussion about setting a maximum line width of 80 characters.

I do not limit code lines to 80 characters. That was a reminiscence of the past when shells were 80 columns wide. Today with modern displays, IDEs and diff tools, even when splitting one window in two editors side by side, it is not that inconvenient to scroll horizontally the few times you need to.

Do limit comments to 80 characters max. Comments should be read in full without the need to scroll sideways.

That said, keep long lines to a minimum. It is best to keep your lines short when it helps with readability. For example, the declaration of an `enum` type with a long list of values might be easier to read when each value uses a single line.

Do:

```
enum weekdays {
    Monday,
    Tuesday,
    Wednesday,
    Thursday,
    Friday,
    Saturday,
    Sunday
};
```

Don't:

```
enum weekdays {Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday};
```



## Variables

### Declaration

Local variables shall be declared where they are used. But consider declaring them at the beginning of the scope if it clearly improves readability.

Global variables and constants shall be declared on a header file.

One declaration at a time. Do not use `,` to declare multiple variables.

Do:

```
uint8_t * my_pointer;
uint8_t my_variable;
```

Don't:

```
uint8_t * my_pointer, my_variable;
```

Same holds true when declaring *struct* fields.

### Fixed-width data types

Avoid using `char`, `short`, `int`, `long`, `long long` and their `signed`and `unsigned` modifiers.

Always use fixed-width data types: `int8_t`, `uint8_t`, `int16_t`, etc. These data types are included in most toolchains. If missing, define them in a **target-specific** header file.

The use of `char` is accepable when declaring strings to use them as such. I.e., when manipulating text. Never use `char` as bytes or integers (use the appropriate fixed-width data type instead).

### Variable scope

Declare local variables in the scope they are used.

Avoid global variables when possible.

Sometimes global variables are required for shared resources when they cannot be passed as arguments. Depending on the toolchain, ISRs are sometimes a place where global variables are needed. Give them very specific names then.

Global constants are acceptable when they are intended to be unique and used project-wide. E.g. `const uint32_t BUILD_NUMBER = 2734`.

Local constants are preferred then they are specific to a given context and they are not a value that could be "tuned".

### `const` keyword

Consider using `const` when declaring variables that cannot change after initialization and call-by-reference function parameters that cannot be modified.

Use of `const` is preferred for typed numerical literals (instead of `#define` macros).

### `static` keyword

Always use the `static` keyword to keep functions and global variables or constants from being used outside their intended modules if appropriate. This increases encapsulation, lowering the risk of name collisions.

### `volatile` keyword

Remember to use `volatile` when:
1. Declaring pointers to peripheral registers.
2. Declaring global variables used by ISRs.
3. Declaring global variables accessed by more than one task.

This is not a recommendation, it is a must. Not using the `volatile` keyword where needed creates hard-to-find bugs.



## Operators

### Styling

Use spaces around binary (with two operands) operators, including the assignment operator (`=`).

Do not use a space between unary operators and their operand.

Do:

```
is_dog_awake = movement_detected && (legs_count == 4);
++iterations;
```

Don't:

```
is_dog_awake=movement_detected&&(legs_count==4);
++ iterations;
```

### Operator precedence

Explicitly indicate precedence using parenthesis. Do not rely on the compiler for operator precedence. Parenthesis surrounding the whole expression are not necessary.

Do:

```
my_best_variable = (cute_number * 2) + 1;
```

Don't:

```
my_best_variable = cute_number * 2 + 1; // Do not rely on compiler for precedence
my_best_variable = ((cute_number * 2) +1); // Unnecessary outermost parenthesis
```

### Bit-wise operators

Bit-wise operators shall be used on unsigned integer data type whose meaningful information are the bits themselves, and not a numerical value.

To operate with numerical values, math operators are strongly preferred.

Do:

```
halved_score = current_score / 2; // Divide by two using a math operator
remainder = whole_value % 256; // Using remainder operator1
```

Don't:

```
halved_score = current_score >> 1; // Divide by two using bit shifting
remainder = whole_value & 0xff; // Using bit-wise and
```



## Numeric literals

Avoid using numeric literals, also called magic numbers.

Use constants or `#define` macros to give literals a name. Give them a long explicit name following the naming conventions.

Sometimes numeric literals make the code more readable when their value does not represent a magnitude (it does not have units). A few examples:

1. Checking against `0`.
2. Setting bits on bitwise operations (`1 << [N]`).
3. When the operation could only make sense using that operand. E.g., dividing in half (`[N] / 2`) or checking parity (`[N] % 2`).
4. When accessing arrays with a fixed offset. E.g., accessing the last element of an array (`my_array[my_array_size -1]`).

Think of readability when using numeric literals and **always** use comments to explain their meaning.



## Comments

### Comment delimiters

Use multi-line comments (`/* ... */`) when commenting blocks of code.
When commenting a block of statements, always place the comment in a new line before the starting statement.

Opening `/*` and closing `*/` must be on their own line.

```
/*
This is a multi-line comment.
This comments take multiple lines.
*/
```

If decorating a multi-line comment, do not draw borders other than the left one. Time is wasted re-aligning other borders every time the comment is modified.

Do:

```
/**
 * This is also a multi-line comment.
 * This comment is easy to edit.
 **/
 ```

Don't:

```
/******************************************
 * Time is wasted if this comment changes *
 ******************************************/
```

Use single-line comments (`// ...`) when commenting one line of code.

When commenting a statement, it is best to place the comment at the end of the line. If the comment does not fit in the same line, place the comment on a new line **before** the statement. Do not mix them within the same block of statements.

Do:

```
uint8_t current_page; // Current page
uint8_t word_count = 0; // Word count on the current page

// Also good:

// Last lap time in seconds
uint32_t last_lap_time; 
// Name of the current track
char current_track_name[] = "The Best Track";
```

Don't:

```
int8_t attendant_count; // Number of people counted
// Average height of attendants
double average_attendant_height;
```

### Grammar is important

Use proper grammar, capitalization, punctuation, etc. to make your comments easy to read.

### Documentation

Place documentation comments on resource (function, variable, etc.) declaration. For functions and global variables, this is their header file. For local variables, where they are declared within their function.

Explain every argument and return value on function documentation. Include "private" (if there is such a thing in C) functions in your documentation, and do not leave them undocumented.

### Comment headers

Include a comment header at the beginning of your files explaining what they are, but **keep it short and to the point**. 

Including copyright information is encouraged. Clearly estate the copyright holder and the license used. A couple lines should suffice. Under no circumstances any content from the license shall be included in the comment header. Include the full license text in the `COPYRIGHT` file in the project root folder.

My standard comment header:

```
/**
 * main.c - My project main file
 *
 * These are optional lines to explain what
 * this file does
 *
 * (C) 2020 Copyright holder name
 * This code is licensed under the COOLEST License.
 */
 ```

Including author name and email is common, but I do not see it useful and do not recommend it anymore. Any information regarding author, changes, contributions and such is expected to be found much more reliably in the version control system.

### `TODO` and `FIXME` comments

These type of comment keywords can be useful during development.

Standardize them and give them specific meaning and use-case. `TODO` and `FIXME` are my keywords of choice.

Make sure you use an automatic tool keeping track of these sections of code allowing you to locate them quickly.

However, **these keywords are strictly forbidden on production code**.

### Disabling code using comments

Disabling sections of code using comments shall be avoided in production code.

When used during development, clearly explain why you decided to maintain that section of code. It can also be paired with a keyword (`TODO`, `FIXME`...) to allow quick location and removal before release.

If you think the disabled code could be useful for future development, it belongs on the documentation of the code with instructions or the `templates` folder.



## Functions

### Styling

1. Function names are all-lowercase, separating words with underscores.
2. There is no space between the function name and the arguments list.
3. There is no space between the opening parenthesis and the type of the first argument, or between the last argument name and closing parenthesis.
4. Commas separating arguments have no spaces before them, and a single space after.
5. Curly braces are always on column 0 in their own new line.

```
void do_something(uint8_t arg1, char * arg2, double arg3)
{
    some_code();
}
```

6. When the list of arguments is too long, break it into different lines. These new lines must start in a column to the right of the opening parenthesis to highlight the function name, and aligned to indentation size.

```
void do_something_else(uint8_t arg1, char * arg2,
                        double arg3, int32_t arg4)
{
    some_other_code();
}
```

### Length

As a general recommendation, keep functions short and to the point. Break down long functions into smaller sub-routines when possible.

Many style guides set the hard limit of one page for how long functions should be, but I do not feel comfortable doing so. Use your best judgement, always putting readability first.

### Return codes

Always check values returned by function calls.

Return codes are more readable when defined using macros or `enum` types.

Define a type for error return values. That way it is easier to understand if a given function returns an error status or a value that is meant to be used. This can be as easy as `typedef int32_t error_t`, for example.

### Interrupt Service Routines (ISR)

Keeping ISR handlers as short as possible is essential. Ideally, an ISR will off-load to a regular task as much as possible.

*Critical Sections* are parts of code with exclusive use of the processor, disabling the scheduler and interrupts, in order to access a shared resource safely. Keep them to a minimum and always as short as absolutely necessary.



## Conditionals

### Styling

1. Start with a first `if` keyword, followed by opening parenthesis (`(`), the condition, closing parenthesis (`)`), a space and opening braces ( `{ `).
2. Write statements in their own new lines.
3. Use a new line for the closing braces (`}`).
4. Any `else` or `else if` starts after the previous closing braces (`}`) preceded by a space.
   * In case of `else if` same rules as for the first `if` apply.
5. Final closing braces are always in their own new line.

```
if(this_variable > 0) {
    call_this_function();
} else if(this_variable < 0) {
    call_this_other_function();
} else {
    ++this_variable;
}
```

### Use of braces

Always wrap statements in curly braces.

Do:

```
if(this variable > 0) {
    call_this_function();
}
```

Don't:

```
if(this_variable > 0) call_this_function();

if(this_variable > 0)
    call_this_function();
```

### Performing actions in conditions

Conditions shall not make changes on variables or the state of the program.

Do:

```
if(!strcmp(stringA, stringB)) { // strcmp() does not change the strings
    return;
}

int32_t err = do_something_that_changes_things();
if(err == SUCCESS) {
    return;
}
```

Don't:

```
if(do_something_that_changes_things() == SUCCESS) {
    return;
}
```



## Other

### Endianness

Do not rely on the compiler to serialize and de-serialize data types. Not all architectures will serialize words in the same order of bytes, and compiling your code in different architectures might give you different results, introducing bugs and making your code less portable.

Do:

```
uint32_t some_32bit_value = get_random_value();
uint8_t serialized_data[4];

// Sending bytes from LSB to MSB
serialized_data[0] = some_32bit_value & 0xff;
serialized_data[1] = (some_32bit_value & 0xff00) >> 8);
serialized_data[2] = (some_32bit_value & 0xff0000) >> 16);
serialized_data[3] = (some_32bit_value & 0xff000000) >> 24);
```

Don't:

```
uint32_t some_32bit_value = get_random_value();
uint8_t serialized_data[4];

// Relying on compiler to serialize data
memcpy(serialized_data, &some_32bit_value, sizeof(some_32bit_value));
```

### 3rd-party code

When using 3rd-party code, such as libraries, HAL layers, etc., use them in their original form, even if it does not fit the style of your code.

For example, FreeRTOS uses variables and function names such as `pvPortMalloc()`, which does not follow this style guide. Just use it the way it was meant to be used without worrying to mix it with your other code.