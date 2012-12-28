# Apron

Apron is a mocking framework for Bash. Why Apron? Because Smock seemed
way too obvious.

## Installation

You should not try to use Apron from a shared location. Simply include
it with your project and source it from a controlled location.

## How to Use

### Mocks

Source apron as early as possible, and call `APRON_enable`. This initializes a
small amount of bookkeeping needed to intercept calls and keep track of any
mocks which you might register. At this point, any call which cannot be
resolved by the shell will be caught by Apron as an uninteresting call. You can
enable Apron with the `-v` flag to get verbose output as to what Apron is
doing.

Next, define your mocks! Let's call one `badcommand`. Of course, at this point,
since shell functions take priority over external commands, the function will
always be called. However, you'll also want to register your mock with Apron,
by calling `APRON_register badcommand`. This allows Apron to keep track of
the function in case you want to selectively unregister it, or temporarily
pause mocking.

Define as many mocks as you want! As mentioned, Apron can temporarily pause
its mocking behavior with `APRON_pause`. This will cause all further calls
to previously mocked commands to be executed normally. You can go back to
your mocked environment with `APRON_unpause`.

Finally, you can tear down your mocks selectively, or all at once. Use
`APRON_unregister` to destroy a single mock or `APRON_unregister_all` to
destroy all known mocks. Unregistering mocks will not cause APRON to be
disabled. To completely uninitialize Apron, call `APRON_disable`. Note that
calling `APRON_disable` will implicitly unregister all mocks for you.

An example of Apron's mocking in use can be found in `mock-test`.

### Expectations

Apron can set and validate expectations on external calls, too. Again, start by
calling `APRON_enable`.  Set expectations by calling `APRON_expect_call` with
the expected external call, and optionally a count. Once you're satisfied, call
the function you want to watch with `APRON_replay`. Once `APRON_replay` returns,
all expected calls are reset.

And of course, you can combine expectations with your mocks to do even closer
validation of behavior.

An example of Apron's expectations in use can be found in `expect-test`.

## TODO/Known limitations

* Add ordering and argument validation to expectations
* Figure out how to mock redirections

## License

See LICENSE for details.
