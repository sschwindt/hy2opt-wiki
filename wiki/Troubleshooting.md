Error messages and Troubleshooting
==================================

***

# Known issues<a name="issues"></a>
We do our very best to program *Hy2Opt* as robust and flexible as possible. *Hy2Opt*'s stepwise development nevertheless led to a few restrictions in its freedom of use, which are caused by a couple of hard-code sequences. The following limitations because of hard-coding are known and will be fixed in future versions:

 - **ISSUE**: Description
 
# How to trace Error and Warning messages<a name="howto"></a>
If *Hy2Opt* encounters problems, the code writes *WARNING* and *ERROR* messages to the *Python* command line and to the same logfile where all other computation progress information is logged. The *WARNING* and *ERROR* messages provide information on the error cause and this Wiki contains all possible *WARNING* and *ERROR* messages of *Hy2Opt*.

Thus, for troubleshooting, press the `CTRL` + `F` key, enter (part of) the *WARNING* or *ERROR* message that occurred, and press enter. *Causes* and *Remedies* are provided for every *WARNING* and *ERROR* message. Do not include specific parameters, file names, or expressions in parentheses or brackets.

Otherwise, the *WARNING* and *ERROR* messages are listed in alphabetic order (starting with **E**rror messages, then **E**xceptionalError message, then **W**arning messages ...).


# Error and Warning message generation

The *Hy2Opt* writes process errors and descriptions to logfiles. When the GUI encounters problems, it directly provides causes and remedies in pop-up infoboxes.
The common error and warning messages, which can be particularly raised by the package (alphabetical order) are listed in the following with detailed descriptions of causes and remedies.
Most error messages are written to the logfiles, but some exception errors are only printed to the terminal because they occur before logging could even be started. Such non-logged `ExceptionErrors` are listed at the bottom of the Error messages.
Some non-identifiable errors raised by the `arcpy` package disappear after rebooting the system.

***

# Error messages

**WILL BE REPLACED WITH TABULAR DATABASE**

 - **`ERROR: Message.`**
    - *Cause:*   text
    - *Remedy:*
        + solution 1
        + solution 2

***

# Warning messages

 - **`WARNING: Message.`**
    - *Cause:*   text.
    - *Remedy:*  solution.
    
 