# Shell Variables

## Printing Variables

FLAG is the name of our variable, and we have to print it. ``echo $FLAG`` 

## Setting Variables

run ``PWN=COLLEGE``, to set the value of the variable PWN as COLLEGE.

## Multi-word Variables

run ``PWN="COLLEGE YEAH"``, with apostrophes due to the space in the command.
We could use \\ to escape the space too 

## Exporting Variables

We have to export the variable PWN, But not COLLEGE. First, set the value of PWN as COLLEGE by doing ``export PWN=COLLEGE``, then set the value of COLLEGE as PWN with ``COLLEGE=PWN``. Get the flag with ``/challenge/run PWN``.

## Printing Exported Variables

run ``env | grep "pwn"``.

## Storing Command Output

We first store the output of /challenge/run into PWN with the command ``PWN=$(/challenge/run)``. Print out PWN with ``cat $PWN`` to get the flag.

## Reading Input

``read -p "Input PWN: " PWN``, ``echo $PWN``

## Reading files

Run ``read PWN < /challenge/run``
