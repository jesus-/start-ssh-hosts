# start-ssh-hosts
start several ssh connections from a host file

```
#!/bin/bash
FILE=$1

if [ -z "$FILE" ]; then
	echo "The first argument needs to be the hosts file"
	 exit 1;
fi
pwd=$(pwd)
counter=0;
position_counter=0;
while read line;
do
	((counter++))
	key="t";
	if [[ $counter -eq 9 ]]; then
		counter=1;
		key="n";
	fi
	name=$(echo $line | cut -f1 -d' ')
	host=$(echo $line | cut -f2 -d' ')
	
	osascript -e " 
	tell application \"Terminal\"" -e "
	tell application \"System Events\" to keystroke \"$key\" using {command down}" -e "
	do script (\"
	printf '\\\e]1;$name\\\a';
	cd $pwd;
	echo \\\"This is the Tab name: $name\\\"
	./deploy $host\") in front window" -e " end tell" 
	
	if [[ $counter -eq 1 ]]; then
		((position_counter++))
		if [[ $position_counter -eq 1 ]]; then
			osascript -e "
			tell application \"System Events\" to key code 123 using {control down, command down}" -e "
			"
		fi

		if [[ $position_counter -eq 2 ]]; then
			osascript -e "
			tell application \"System Events\" to key code 124 using {control down, command down}" -e "
			"
		fi

		if [[ $position_counter -eq 3 ]]; then
			osascript -e "
			tell application \"System Events\" to key code 123 using {control down, shift down, command down}" -e "
			"
		fi

		if [[ $position_counter -eq 4 ]]; then
			osascript -e "
			tell application \"System Events\" to key code 124 using {control down, shift down, command down}" -e "
			"
		fi
	fi

done < $FILE

```
