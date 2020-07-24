# Live Coding Session #14 - Variables and assigments

When: Thursday July 23th, 2020 - 5:00 PM Europe/Berlin

Topic: Looking at variables in recipes and the various forms of operations and assignments that can be used on them. Includes some notes on bbappends and bbclasses, as well as evaluation order.

## Notes from session

### awk magic to see only the evaluation of a specific variable:

    bitbake -e example | awk '/^# \$FOOBAR \[/,/^FOOBAR/'

Will give you the evaluation history of the variable `FOOBAR` in the context of the `example` recipe.

### Note by Chris Larson on _remove:

*if* you need to be able to remove for some reason, you can provide an intermediate variable. FOOBAR_REMOVE ?= "foo"; FOOBAR_remove = "${FOOBAR_REMOVE}"; for example. if you're making a bbappend o remove something, this is a best practice so that additional bbappends after that one can undo the removal. but yes, best to reduce usage entirely

## Recording

[YouTube](https://youtu.be/eFxVqdkh_Kw)

## External resources

[meta-lithium](https://github.com/TheYoctoJester/meta-lithium): the layer created during the session

[Bitbake User Manual](https://www.yoctoproject.org/docs/current/bitbake-user-manual/bitbake-user-manual.html): the manual for bitbake which elaborates on the operation.