#!/bin/bash
# 2018-05-01 in Broken Heart
# Coded by : unix666

# Function colour
RED='\033[0;31m'
CYAN='\033[0;36m'
YELLOW='\033[1;33m'
ORANGE='\033[0;33m' 
PURPLE='\033[0;35m'
GRN='\e[32m'
WHI='\e[37m'
NC='\033[0m'
SECONDS=0
	
echo "

███████╗████████╗ ██████╗ ██████╗ ███╗   ███╗███╗   ███╗ █████╗ ██╗██╗     ███████╗██████╗  
██╔════╝╚══██╔══╝██╔═══██╗██╔══██╗████╗ ████║████╗ ████║██╔══██╗██║██║     ██╔════╝██╔══██╗ 
███████╗   ██║   ██║   ██║██████╔╝██╔████╔██║██╔████╔██║███████║██║██║     █████╗  ██████╔╝ 
╚════██║   ██║   ██║   ██║██╔══██╗██║╚██╔╝██║██║╚██╔╝██║██╔══██║██║██║     ██╔══╝  ██╔══██╗ 
███████║   ██║   ╚██████╔╝██║  ██║██║ ╚═╝ ██║██║ ╚═╝ ██║██║  ██║██║███████╗███████╗██║  ██║ 
╚══════╝   ╚═╝    ╚═════╝ ╚═╝  ╚═╝╚═╝     ╚═╝╚═╝     ╚═╝╚═╝  ╚═╝╚═╝╚══════╝╚══════╝╚═╝  ╚═╝ 

                                                                       V e r s i o n 0.1.3

"

usage() { 
  echo "Usage: ./myscript.sh COMMANDS: [-n <support>] [-e <name@domain.tld>] [-r <name@domain.tld>] [-s <subject>] 
[-u <http://s.id>] [-g <http://s.id/_.html>] [-k {y/n}] [-i <list.txt>] [-a <api.txt>] [-f <folder/>] [-l {1-1000}] 
[-t {1-10}] [-d {y/n}] [-c {y/n}]

Command:
-n (support)                Email form name	
-e (name@domain.tld)        From email auto random paramenter [RANDTX] and [RANDNM]
                            (Ex: support@[RANDNM].spt[RANDTX]e.com)
-r (name@domain.tld)        From replyTo auto random paramenter [RANDTX] and [RANDNM]
                            (Ex: no-reply@[RANDNM].spt[RANDTX]e.com)
-s (subject)                Email subject  auto random parameter [RANDID]
                            (Ex: Notice: Issue ID #[RANDID])
-u (http://s.id)            Link use to replace <#[url]#> in the HTML Letter
-g (http://s.id/_.html)     Letter link to get with file_get_contects or by curl
-k (y/n)                    Encode the email subject and email sender name
-i (20k-US.txt)             File input that contain email to send
-a (api.txt)                File that contain api credential
-r (result/)                Folder to store the result of sended email
-l (60|90|110)              How many list you want to send per delayTime
-t (3|5|8)                  Sleep for -t when send is reach -l fold
-d (y/n)                    Delete the list from input file per send
-c (y/n)                    Compress result to compressed/ folder and
                            move result folder to hassended/
-h                          Show this manual to screen

Report any bugs to: unix666 (https://t.me/unix666)
"
  exit 1 
}

# Assign the arguments for each
# parameter to global variable
while getopts ":n:e:r:s:u:g:k:i:a:f:l:t:g:dch" o; do
    case "${o}" in
    	n)
			from_name=${OPTARG}
			;;
    	e)
			from_mail=${OPTARG}
			;;
    	r)
			reply_to=${OPTARG}
			;;
    	s)
			subject=${OPTARG}
			;;
        u)
            mylink=${OPTARG}
            ;;
        g)
            letter=${OPTARG}
            ;;
        k)
            encode=${OPTARG}
            ;;
        i)
            inputFile=${OPTARG}
            ;;
        a)
            apiFile=${OPTARG}
            ;;
        f)
            targetFolder=${OPTARG}
            ;;
        l)
            sendList=${OPTARG}
            ;;
        t)
            perSec=${OPTARG}
            ;;
        d)
            isDel=${OPTARG}
            ;;
        c)
            isCompress=${OPTARG}
            ;;
        h)
            usage
            ;;
    esac
done

# Asking user whenever the
# parameter is blank or null
# mailer configuration
if [[ $from_name == '' ]]; then
	read -p "[+] Email from name (Ex: Account Support): " from_name
fi

if [[ $from_mail == '' ]]; then
	echo "[+] Email from and reply to auto random paramenter [RANDTX] and [RANDNM]"
	read -p " >> Email from (Ex: support@[RANDNM].spt[RANDTX]e.com): " from_mail
fi

if [[ $reply_to == '' ]]; then
	read -p " >> Email reply to (Ex: no-reply@[RANDNM].spt[RANDTX]e.com): " reply_to
fi

if [[ $subject == '' ]]; then
	echo "[+] Email subject auto random parameter [RANDID]"
	read -p " >> Subject (Ex: Information abaut your account #[RANDID]): " subject
fi

if [[ $mylink == '' ]]; then
	read -p "[+] Link replace <#[url]#> in HTML letter with this link (Ex: http://bit.ly/xGfs): " mylink
fi

if [[ $letter == '' ]]; then
	read -p "[+] HTML Letter (Ex: http://stayevil.net/letter.txt): " letter
fi

if [[ $encode == '' ]]; then
	read -p "[+] Encode the email subject and email sender name (y/n): " encode
fi

if [[ $inputFile == '' ]]; then
	# Print available file on
	# current folder
	# clear
	read -p "[+] Enter email list file: " inputFile
fi

if [[ $apiFile == '' ]]; then
	# Load api file
	read -p "[+] Enter API file: " apiFile
fi

if [[ $targetFolder == '' ]]; then
	read -p "[+] Enter target folder: " targetFolder
	# Check if result folder exists
	# then create if it didn't
	if [[ ! -d "$targetFolder" ]]; then
		echo "[+] Creating $targetFolder/ folder"
		mkdir $targetFolder
	else
		read -p "[+] $targetFolder/ folder are exists, append to them ? [y/n]: " isAppend
		if [[ $isAppend == 'n' ]]; then
			exit
		fi
	fi
else
	if [[ ! -d "$targetFolder" ]]; then
		echo "[+] Creating $targetFolder/ folder"
		mkdir $targetFolder
	fi
fi

if [[ $isDel == '' ]]; then
	read -p "[+] Delete list per send ? [y/n]: " isDel
fi

if [[ $isCompress == '' ]]; then
  read -p "[+] Compress the result ? [y/n]: " isCompress
fi

if [[ $sendList == '' ]]; then
	read -p "[+] How many list you want to send: " sendList
fi

if [[ $perSec == '' ]]; then
	read -p "[+] Delay send in second: " perSec
fi

if [[ ! -d "$targetFolder" ]]; then
	echo "[+] Creating $targetFolder/ folder"
	mkdir $targetFolder
fi

# Define json decoder mailer
function jsondec {
    temp=`echo $json | sed 's/\\\\\//\//g' | sed 's/[{}]//g' | awk -v k="text" '{n=split($0,a,","); for (i=1; i<=n; i++) print a[i]}' | sed 's/\"\:\"/\|/g' | sed 's/[\,]/ /g' | sed 's/\"//g' | grep -w $prop`
    echo ${temp##*|}
}

# Define curl function
doExcecution() {
	header="`date +%H:%M:%S` | $inputFile > $targetFolder"
	footer="[ STORMMAILER ]"

	json=`curl -s $4 --data 'senderEmail='"$from_mail"'&senderName='"$from_name"'&replyTo='"$reply_to"'&subject='"$subject"'&link='"$mylink"'&messageLetter='"$letter"'&emailList='"$1"'&messageType=1&encode='"$encode"'&act=mailer' -X POST`
	prop='error_code'
	mailres=`jsondec`
				
	if [[ $mailres == 'error_code:0' ]]; then
		printf "[$header] $2/$3. Sending email to ${GRN}$1 ${NC}successfully $footer\n"
		echo "$1" >> $targetFolder/SEND-SUCCESS.txt
	else
		if [[ $mailres == 'error_code:2' ]]; then
			printf "[$header] $2/$3. Sending email to ${RED}$1 ${NC}failed $footer\n"
			echo "$1" >> $targetFolder/SEND-FAIL.txt
		else
			if [[ $mailres == 'error_code:-1' ]]; then
				printf "[$header] $2/$3. Sending email to ${CYAN}$1 ${NC}skiped becouse wrong $footer\n"
				echo "$1" >> $targetFolder/SEND-WRONG.txt
			else
				if [[ $mailres == 'error_code:1' ]]; then
					printf "[$header] $2/$3. Sending email to ${PURPLE}$1 ${NC}error [ API: ${RED}$4 ${NC}]\n"
					echo "$1" >> $targetFolder/SEND-ERROR.txt
				else
					printf "[$header] $2/$3. Sending email to ${ORANGE}$1 ${NC}problem [ API: ${RED}$4 ${NC}]\n"
					echo "$1" >> $targetFolder/SEND-PROBLEM.txt
				fi
			fi
		fi
	fi
}

# Preparing file list 
# by using email pattern 
# every line in $inputFile
echo "########################################"
echo "[+] Cleaning your mailist file"
grep -Eiorh '([[:alnum:]_.-]+@[[:alnum:]_.-]+?\.[[:alpha:].]{2,6})' $inputFile | sort | uniq > temp_list && mv temp_list $inputFile

echo "[+] Sender Name : $from_name"
echo "[+] Sender Email : $from_mail"
echo "[+] ReplyTo : $reply_to"
echo "[+] Subject : $subject"
echo "[+] Letter : $letter"
echo "[+] Link : $mylink"
echo "[+] Send $sendList email per $perSec second"
# Finding match mail provider
echo "########################################"
# Print total line of mailist
totalLines=`grep -c "@" $inputFile`
echo "There are $totalLines of list."
echo " "
echo "Aol: `grep -c "@aol." $inputFile`"
echo "Gmail: `grep -c "@gmail." $inputFile`"
echo "Hotmail: `grep -c "@hotmail." $inputFile`"
echo "Outlook: `grep -c "@outlook." $inputFile`"
echo "Yamdex: `grep -c "@yandex." $inputFile`"
echo "Yahoo: `grep -c "@yahoo." $inputFile`"
echo "########################################"

IFS=$'\r\n' GLOBIGNORE='*' command eval  'mailist=($(cat $inputFile))'
IFS=$'\r\n' GLOBIGNORE='*' command eval  'api=($(cat $apiFile))'
con=1
apiLen="${#api[@]}"

for (( i = 0; i < "${#mailist[@]}"; i++ )); do
	targetmail="${mailist[$i]}"
	indexer=$((con++))
	tot=$((totalLines--))
	url="${api[$i % apiLen]}"
	fold=`expr $i % $sendList`
	if [[ $fold == 0 && $i > 0 ]]; then
		duration=$SECONDS
		#echo "[+] Waiting $perSec second on $i list."
		sleep $perSec
	fi

	doExcecution "$targetmail" "$indexer" "$tot" "$url" &

	if [[ $isDel == 'y' ]]; then
		grep -v -- "$targetmail" $inputFile > "$inputFile"_temp && mv "$inputFile"_temp $inputFile
	fi
done

# waiting the background process to be done
# then checking list from garbage collector
# located on $targetFolder/unknown.txt
echo "[+] Waiting background process to be done"
wait
wc -l $targetFolder/*

if [[ $isCompress == 'y' ]]; then
	tgl=`date`
	tgl=${tgl// /-}
	zipped="$targetFolder-$tgl.zip"

	echo "[+] Compressing result"
	if [[ ! -d "compressed" ]]; then
		mkdir "compressed"
	fi
	zip -R "compressed/$zipped" "$targetFolder/SEND-SUCCESS.txt" "$targetFolder/SEND-FAIL.txt" "$targetFolder/SEND-WRONG.txt" "$targetFolder/SEND-ERROR.txt" "$targetFolder/SEND-PROBLEM.txt"
	echo "[+] Saved to compressed/$zipped"
	mv $targetFolder hassended
	echo "[+] $targetFolder has been moved to hassended/"
fi
echo "+==========+ STORMMAILER : Coded By unix666 +==========+"

