#zsh
## save command output to variable
	name=`ls ~/Documents`
	name=$(ls ~/Documents)
	echo $name

## foreach
	zsh% foreach f (*.cc)
	foreach> echo $f
	foreach> end
	
## splite string
	string="1::3"
	a=(${(s/:/)string})
	echo $a[1] # 1
	echo $a[2] # 3 ?? I want an empty string, as in Python
The solution is

	a=("${(s/:/)string}") # notice the quotes

	echo $a[1] # 1, good
	echo $a[2] # nothing, good
	echo $a[3] # 3, good
	
### For explernation [Reference](http://unix.stackexchange.com/questions/28854/list-elements-with-spaces-in-zsh)

First, I assume that the use of `ls` is just an example. You cannot parse the output of `ls` in any shell, because it is ambiguous. Read [Why you shouldn't parse the output of ls(1)](http://mywiki.wooledge.org/ParsingLs) if this is news to you. In any shell, to obtain a list of files, use wildcards, `e.g. files=(*)`.

In zsh, like in other shells, the result of [command substitution](http://zsh.sourceforge.net/Doc/Release/Expansion.html#Command-Substitution) is split into words at whitespace characters (more precisely, according to the value of `IFS`). (Unlike other shells, the result of command substitution is not subject to globbing in zsh.) So if the output of the `ls` command is

	hello world
	wibble
then `files=($(ls))` sets the files array to contain 3 elements: `hello`, `world` and `wibble`.

If the command substitution is in double quotes, then no splitting is performed. You can perform custom splitting with [parameter expansion flags](http://zsh.sourceforge.net/Doc/Release/Expansion.html#Parameter-Expansion-Flags). Use the `@` flag to indicate that the result of the splitting is to be an array (oddly, you need to keep the expansion in double quotes, i.e. `"${(@)…}"`, even though the double-quoted string will expand to multiple words). For splitting, use the `s` flag, e.g. `"${(@s:,:)…}"` to split at commas; the `f` flag splits at newlines only.

	files=("${(@f)$(ls)}")
Note that the proper way to iterate over an array in general is `for f in $files[@]`, as `$files` strips off empty elements (here, it doesn't matter because the elements won't be empty).

`print $f` interprets `$f` as a switch if it begins with a `-` and expands backslashes in `$f`. Use `print -r -- $f`, or `print -rn -- $f` if you don't want to add a newline after the string.