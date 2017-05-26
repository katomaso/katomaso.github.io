# Open and Closed form of Control Structure IF

When writing if prefer "closed" form instead of "open" form.

  # open form
  if some_condiditon:
    do_something()

  # closed form
  if not some_condition():
    return
  do_something()

Closed form gives you clear idea which parts of code gets executed under what
circumstances.

**Open forms** naturally lead to deeper code which is unreadable for ordinary humans
like me. Imagine more conditioned example. Are you able to state what are
necessary conditions for do_C and do_D to run?

  # deep open form
  if condition_A:
    do_A()
    if condition_B:
      do_B()
    else:
      if condition_C:
        do_C()
    do_D()

**Closed form** of the same code encourages flat structure and returning / breaking
the code early. I decided to repeat do_D() in the last two ifs for the sake of
readability. Once you extract code in a function it is not a crime to call the
function on many places. That is why you extract functionality into functions.

  if not condition_A:
    return
  do_A()

  if condition_B:
    do_B()
    do_D()
    return

  if condition_C:
    do_C()
    do_D()
    return

Compare again your ability to state under which conditions do_C() and do_D() run.


Some rules of thumb how to spot open forms

1.  Your IFs are being folded too much
1.  You have many (any) ELSEs
1.  [When I Wrote It, Only God and I Knew the Meaning; Now God Alone Knows][quote]

[quote]: http://quoteinvestigator.com/2013/09/24/god-knows/