Download Link: https://assignmentchef.com/product/solved-csci-1200-data-structures-homework-5-radiods-song-grouping
<br>
<strong>RadioDS </strong>is a new radio station that is trying to get their library organized. The station has made a deal with a music company, and is able to use a large collection of songs, called a Library. However, in order to keep the licenses, <strong>RadioDS </strong>has been told that they cannot play any one song too frequently. Instead of keeping an exact count, the station has decided it’s good enough if they break up the library into di↵erent Song Groups. A given song can be in 0 or 1 Song Groups at at any time. If a sta↵ member decides they want a song that’s currently in Song Group 1 to be used in Song Group 2 instead, they must first remove the song from Song Group 1.

To stand out from the many competing stations, <em>RadioDS </em>has two speciality radio programs. One is called an “Artist Marathon”, where all the songs played will have the same artist. The other program is called a “Rewind List”, where every song played is from an earlier year than the song before it. Examples of both programs are placed later in this handout.

Before beginning this assignment, make to <strong>read the entire handout</strong>. There are several classes involved in the operation of <em>RadioDS</em>, and it is important to understand how they interact in order to succeed on this assignment. In addition, many of the things described in this handout are already implemented and provided, so if you are not careful you may be doing more work than you should!

<h1>Classes and Data Organization</h1>

There are three classes used in this assignment.

Song is a simple class that contains the metadata (information) about a song. There are four distinct attributes used to identify songs: the <em>title </em>of the song, the <em>artist</em>, the <em>year </em>the song came out, and the <em>album </em>the song was on. The artist is the band or person(s) who made the song. An album is typically a CD or vinyl record, but it may represent a di↵erent collection. We will not focus on more advanced metadata, but if you are curious about how complicated describing a song can get in the real world, you may find this link to be an interesting read: <a href="https://musicbrainz.org/doc/Terminology">https://musicbrainz.org/doc/Terminology</a>

Each Song also has a boolean flag to indicate whether it has been used in a Song Group, and a pointer to a SongGroupNode, described later in this section. If there is no SongGroupNode associated with this Song then the pointer should be set to NULL. It will be important to keep these variables updated throughout your code.

To provide a singly-linked list, we have included the templated Node&lt;T&gt;, similar to what has been seen in class. Each Node holds a value and has a pointer ptr that points to the next node. There is no special “head” node; the list is either empty or it contains one or more Node objects linked together through forward pointers.

The Library is represented by a singly-linked list of type Node&lt;Song&gt;*. Your implementation should never copy Song objects out of the Library. Instead we will use pointers in other classes to decrease how often we have to search through the Library. Below is an example of a Library with simplified Song objects (we do not include the bool or SongGroupNode*. We include memory addresses of the Node&lt;Song&gt;* to help illustrate the other structures.

The other concept we have to represent is Song Groups. Before we can do so, we need to introduce a complicated linked list node type called SongGroupNode. Two of the pointers in each node should look familiar – namely <em>prev song </em><em>ptr </em>and <em>next song </em><em>ptr</em>. These two pointers allow us to traverse a Song Group like any other doubly-linked list, so maintaining them should be familiar if you have been following along with lecture and lab. Neither pointer has anything to do with the order in the Library, they instead reflect the order that Song objects were added to the Song Group. The <em>next </em><em>by artist ptr </em>should point to the next SongGroupNode in the Song Group with the same artist. Similarly, the <em>prev by year ptr </em>should point to a previous SongGroupNode in the Song Group which has an earlier year than the current SongGroupNode.

Before illustrating what a Song Group looks like, there are two more members in the SongGroupNode that need to be described. To tell Song Groups apart, each Song Group is assigned a unique identification number (ID), which is stored in the <em>id </em>member of every SongGroupNode. The final member is <em>m </em><em>song</em>, which points to a Node&lt;Song&gt;* – in other words it points to the Library Node that contains the Song represented in the corresponding SongGroupNode. Below is a picture of a SongGroup, with addresses from the Library illustrated above instead of replicating all the Song members:

There is one final part to the organization of data, and that is how to keep track of Song Groups. Since we need to know the beginning of each Song Group, we maintain a list of “group heads” in a singly-linked list of Node&lt;SongGroupNode*&gt;* objects. Each Node points to the first SongGroupNode in a Song Group. This introduces an interesting quirk to how we represent Song Groups though – there will always be at least one SongGroupNode for a given group ID. When the group is “empty”, the first SongGroupNode will simply set <em>m </em>song to NULL. An example of the group head list, along with an illustration of an empty, one song, and two song Song Group is shown to the left.

<h1>Execution and Input Commands</h1>

The program will read in a library file, and then read in a series of commands. Since the syntax of input commands is less important (the code to read in commands is already written for you), we will only describe them briefly.

Execution looks something like:

./radioDS.out TinyLibrary.txt input.txt out_library.txt out_songgroup.txt out_print.txt

The first argument is a file that contains the Library data. The second argument is a file contains a list of commands, which you can assume are always formatted correctly. The third file is where output for libraryrelated commands will be saved. The fourth file is similarly a file for output from Song Group commands, and finally the fifth file holds output from “print” commands. The commands are described below, and grouped by which file their output goes to.

<h2>Library Commands</h2>

add to library adds a new Song to the Library

next in library attempts to find a Song and print the Song that was added to the Library immediately after it song exists prints out whether or not a Song is in the Library.

<h2>Song Group Commands</h2>

make group creates a Song Group with a particular ID if that group doesn’t already exist. group exists prints out whether or not a Song Group exists.

song in group prints out whether or not a particular Song is in a specific Song Group. add to group attempts to add a Song to a specific Song Group. remove from group attempts to remove a Song from the Song Group it’s in.

remove group removes a Song Group from the list of groups, and marks any Song that was in it as unused.

<h2>Print Commands</h2>

print library prints every Song in the Library in the order it was added. print group sequential prints every Song in a Song Group in the order that the songs were added.

print group rewind prints a Rewind List starting from (and including) a particular Song. print group marathon prints an Artist Marathon starting from (and including) a particular Song.

print longest group rewind finds the longest Rewind List in a given Song Group, and prints out the Rewind List along with its length.

<h1>Your Task</h1>

Unlike previous homework assignments, the parsing is already complete, and so is the logic to handle the commands. The parsing and input handling logic is in <em>main.cpp</em>, and this file should not be changed. Similarly, none of the typedefs, class definitions, or prototypes in <em>SongLibrary.h </em>should be changed. We still recommend you read through both files to understand what’s happening. <em>main.cpp </em>also handles all the Library functions, and you may find the code that is already written helpful in figuring out how to manage singly-linked lists.

You will need to complete the implementation of <em>student </em><em>functions.cpp </em>file. A few functions at the top of the file should not be changed. You should only change the implementation of functions that are empty. Each of the functions you should implement has a comment block above it which has some information about the arguments being passed in, the expected behavior of the function, and the return values. Between the description in the write-up, the code in <em>main.cpp</em>, and the comments in <em>student </em><em>functions.cpp</em>, you should be able to figure out what each function should do.

You are not allowed to use STL vector or STL list objects. You may not write any additional classes or structs. You should make sure the program does not run with any memory leaks or memory errors when compiled with your implementation.

While you are encouraged to write your own test input, we will not be looking at your inputs. You should submit only a completed <em>README.txt </em>and your completed <em>student functions.cpp</em>.

<h1>Helpful Hints</h1>

Linked lists can get messy quickly, so we advise looking at small examples and drawing pictures when your code is behaving strangely. Update your picture as you follow your code line-by-line and see if something in the drawing doesn’t end up the way you expected it to. Drawing pictures will be helpful in understanding the various data structures and may help you as you plan out your implementation.

A technique that will be of particular use is getting the address of a variable on the stack using &amp;. For example, consider the following code:

int x = 5; int* y = &amp;x; Node&lt;int*&gt; n;

n.value = y; //Now n.value is 5 *(n.value) = 10; //Now x is 10

Some sample files have been provided to give you an idea of how the output looks and start you o↵ with some simple test inputs. These are not thorough, but they do test a few things that you may not have thought about. The library <em>library </em><em>small.txt </em>is used for <em>input basic.txt</em>, <em>input basicsong.txt</em>, and <em>input moregroups.txt </em>while <em>input radio.txt </em>is designed to work with <em>library </em><em>medium.txt</em>