# davopslab
............>ansible.......
cd /etc/yum.repos.d/
ll
vi amzn2-core.repo
%s/enabled=0/enabled=1/g

enabling epel for amazon linux
sudo yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
ansible and press tab to see 
cd /etc/ansible/
ll
-rw-r--r-- 1 root root 20277 Jan 18 01:34 ansible.cfg
-rw-r--r-- 1 root root  1016 Jan 18 01:34 hosts
drwxr-xr-x 2 root root     6 Jan 18 01:34 roles






creating ec2 instance

https://www.bogotobogo.com/DevOps/Ansible/Ansible-aws-creating-ec2-instance.php






  TOmcat Playbook
1.http://myblogmchopker.blogspot.com/2017/08/ansible-tomcat-installation-deployment.html



---------------------------------------------------------------------------------------------------------
----------- Docker-------------------- 

cd /etc/yum.repos.d/
ls -ltr
vi redhat-rhui.repo
:set nu
%s/enabled=0/enabled=1/g
(small s only) then wq!
yum update -y
yum install docker -y
systemctl start docker
ps -ef | grep -i docker
cd
mkdir -p /opt/devops/docker
cd /opt/devops/docker/
mkdir lab1
cd lab1/
echo "This is test docker" >> index.html
vi dockerfile
FROM nginx:alpine
COPY . /usr/share/nginx/html
cat dockerfile
ls -ltr
docker build -t nginxlab:1 .



creating one new imaage and pulling that image to docker hub
docker pull ubuntu
docker images
docker run -it -d ubuntu
docker ps -a
docker exec -it ddc39c83fc24 bash
ls
mkdir test-directorry
exit
docker ps -a
docker commit c718a1c0ccca msreddy621/srinivasreddy:7
docker push msreddy621/srinivasreddy:6

A problem rectified to push image from locl to central repo by this
https://bugzilla.redhat.com/show_bug.cgi?id=1434897

to build job  with docker 
https://www.upnxtblog.com/index.php/2018/02/02/build-docker-images-using-jenkins/



docker login docker.io

  more /etc/sysconfig/docker
  164  cd /etc/docker/
  165  ls -lt
  166  grep -i redhat *
  167  grep -iR redhat *
  168  more /etc/containers/registries.conf
  169  vi /etc/containers/registries.conf
-----------------------------------------------------------------------------------------------

cd /tmp/
yum install wget unzip -y
wget -c --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u131-b11/d54c1d3a095b4ff2b6607d096fa80163/jdk-8u131-linux-x64.rpm
rpm -ivh jdk-8u131-linux-x64.rpm
wget http://mirrors.estointernet.in/apache/tomcat/tomcat-8/v8.5.37/bin/apache-tomcat-8.5.37-windows-x64.zip
unzip apache-tomcat-8.5.37-windows-x64.zip
mv apache-tomcat-8.5.37 /opt/tomcat
chmod -R 755 /opt/tomcat/
rm -rf /opt/tomcat/webapps/*
/opt/tomcat/bin/startup.sh
ps -ef | grep tomcat
wget https://updates.jenkins-ci.org/download/war/2.156/jenkins.war
cp jenkins.war /opt/tomcat/webapps/
wget http://mirrors.estointernet.in/apache/maven/maven-3/3.6.0/binaries/apache-maven-3.6.0-bin.zip
unzip apache-maven-3.6.0-bin.zip
mv apache-maven-3.6.0 maven
yum install git -y
------------------------------------------------------------------------------------------------------------------------------------

==========K8============

1) first install the instance (take rhel7.5)

2) enable the EPEL for kubernetes

cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=http://yum.kubernetes.io/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg
https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF

3) yum update -y

4) install the kubernetes

yum install kubelet kubeadm kubectl kubernetes-cni -y --nogpgcheck

5) start and enable the kubernet
systemctl enable kubelet 
systemctl start kubelet


6) disable the selinux

  vi /etc/selinux/config

go to 7th line and edit (permanently diable) SELINUX=disabled


7)-----------no need to do this step----- stop and disable th firewall 
( there is no firewalld package in rhel7 no need to disable and stop the firewalld)

systemctl disable firewalld 
systemctl stop firewalld

8)enable the EPEL for docker 

   vi /etc/yum.repos.d/redhat-rhui.repo
    esc :%s/enabled=0/enabled=1/g

9) install the docker

yum install docker -y

systemctl start docker

systemctl enable docker

10) use below commands to run 

sysctl -w net.bridge.bridge-nf-call-iptables=1 

echo "net.bridge.bridge-nf-call-iptables=1" > /etc/sysctl.d/k8s.conf




11) from instance reboot the server

12) take the 0wn ami for master 

13) run the command
 kubeadm init


  kubeadm join 172.31.22.170:6443 --token ne8lk6.pgf5tng78tna3sc4 --discovery-token-ca-cert-hash sha256:a023ae74566113a008b9c2f7ea87f3fc253198909aa70ca94765f85cc5830068



14)
 mkdir -p $HOME/.kube
 cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
 chown $(id -u):$(id -g) $HOME/.kube/config

and launch 3 slave nodes

16)Deploy pod network to the cluster

kubectl get nodes
kubectl get pods --all-namespaces

15)Run the beneath command to deploy network.

export kubever=$(kubectl version | base64 | tr -d '\n')
kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$kubever"


-------------------------------------------------------------------------------------------------------------------

1) How to pass argument to a script ?
./script argument

Example : Script will show filename

./show.sh file1.txt

cat show.sh
#!/bin/bash
cat $1

2) How to use argument in a script ?
First argument: $1,
Second argument : $2

Example : Script will copy file (arg1) to destination (arg2)

./copy.sh file1.txt /tmp/

cat copy.sh
#!/bin/bash
cp $1 $2

3) How to calculate number of passed arguments ?
$#

4) How to get script name inside a script ?
$0

5) How to check if previous command run successful ?
$?

6) How to get last line from a file ?
tail -1

7) How to get first line from a file ?
head -1

8) How to get 3rd element from each line from a file ?
awk '{print $3}'

9) How to get 2nd element from each line from a file, if first equal FIND
awk '{ if ($1 == "FIND") print $2}'

10)  How to debug bash script
Add -xv to #!/bin/bash

Example

#!/bin/bash –xv

11) Give an example how to write function ?
function example {
echo "Hello world!"
}

12) How to add string to string ?
V1="Hello"
V2="World"
let V3=$V1+$V2
echo $V3

Output

Hello+World

13) How to add two integers ?
V1=1
V2=2
V3=$V1+$V2
echo $V3

Output
3

Remember you need to add "let" to line V3=$V1+$V2
then echo $V3 will give 3

if without let , then it will be

echo $V3 will give 1+2

14) How to check if file exist on filesystem ?
if [ -f /var/log/messages ] then
echo "File exists"
fi

15) Write down syntax for all loops in shell scripting ?
for loop :

for i in $( ls ); do
echo item: $i
done

while loop :

#!/bin/bash
COUNTER=0
while [ $COUNTER -lt 10 ]; do
echo The counter is $COUNTER
let COUNTER=COUNTER+1
done

until loop :

#!/bin/bash
COUNTER=20
until [ $COUNTER -lt 10 ]; do
echo COUNTER $COUNTER
let COUNTER-=1
done

16) What it means by #!/bin/sh or #!/bin/bash at beginning of every script ?
That line tells which shell to use. #!/bin/bash script to execute using /bin/bash. In case of python script there there will be #!/usr/bin/python

17) How to get 10th line from the text file ?
head -10 file|tail -1

18) What is the first symbol in the bash script file
#

19) What would be the output of command: [ -z "" ] && echo 0 || echo 1
0

20) What command "export" do ?
Makes variable public in subshells

21) How to run script in background ?
add "&" to the end of script

22) What "chmod 500 script" do ?
Makes script executable for script owner

23) What ">" do ?
Redirects output stream to file or another stream.

24) What difference between & and &&
& - we using it when want to put script to background
&& - when we wand to execute command/script if first script was finished successfully

25) When we need "if" before [ condition ] ?
When we need to run several commands if condition meets.

26) What would be the output of the command: name=John && echo 'My name is $name'
My name is $name

27) Which is the symbol used for comments in bash shell scripting ?
#

28) What would be the output of command: echo ${new:-variable}
variable

29) What difference between ' and " quotes ?
' - we use it when do not want to evaluate variables to the values
" - all variables will be evaluated and its values will be assigned instead.

30) How to redirect stdout and stderr streams to log.txt file from script inside ?
Add "exec >log.txt 2>&1" as the first command in the script

31) How to get part of string variable with echo command only ?
echo ${variable:x:y}
x - start position
y - length
example:
variable="My name is Petras, and I am developer."
echo ${variable:11:6} # will display Petras

32) How to get home_dir with echo command only if string variable="User:123:321:/home/dir" is given ?
echo ${variable#*:*:*:}
or
echo ${variable##*:}

33) How to get “User” from the string above ?
echo ${variable%:*:*:*}
or
echo ${variable%%:*}

34) How to list users which UID less that 100 (awk) ?
awk -F: '$3<100' /etc/passwd

35) Write the program which counts unique primary groups for users and displays count and group name only
cat /etc/passwd|cut -d: -f4|sort|uniq -c|while read c g
do
{ echo $c; grep :$g: /etc/group|cut -d: -f1;}|xargs -n 2
done

36) How to change standard field separator to ":" in bash shell ?
IFS=":"

37) How to get variable length ?
${#variable}

38) How to print last 5 characters of variable ?
echo ${variable: -5}

39) What difference between ${variable:-10} and ${variable: -10} ?
${variable:-10} - gives 10 if variable was not assigned before
${variable: -10} - gives last 10 symbols of variable

40) How to substitute part of string with echo command only ?
echo ${variable//pattern/replacement}

41) Which command replaces string to uppercase ?
tr '[:lower:]' '[:upper:]'

42) How to count local accounts ?
wc -l /etc/passwd|cut -d" " -f1
or
cat /etc/passwd|wc -l

43) How to count words in a string without wc command ?
set ${string}
echo $#

44) Which one is correct "export $variable" or "export variable" ?
export variable

45) How to list files where second letter is a or b ?
ls -d ?[ab]*

46) How to add integers a to b and assign to c ?
c=$((a+b))
or
c=`expr $a + $b`
or
c=`echo "$a+$b"|bc`

47) How to remove all spaces from the string ?
echo $string|tr -d " "

48) Rewrite the command to print the sentence and converting variable to plural: item="car"; echo "I like $item" ?
item="car"; echo "I like ${item}s"

49) Write the command which will print numbers from 0 to 100 and display every third (0 3 6 9 …) ?
for i in {0..100..3}; do echo $i; done
or
for (( i=0; i<=100; i=i+3 )); do echo "Welcome $i times"; done

50) How to print all arguments provided to the script ?
echo $*
or
echo $@

51) What difference between [ $a == $b ] and [ $a -eq $b ]
[ $a == $b ] - should be used for string comparison
[ $a -eq $b ] - should be used for number tests
52) What difference between = and ==
= - we using to assign value to variable
== - we using for string comparison

53) Write the command to test if $a greater than 12 ?
[ $a -gt 12 ]
54) Write the command to test if $b les or equal 12 ?
[ $b -le 12 ]
55) How to check if string begins with "abc" letters ?
[[ $string == abc* ]]
56) What difference between [[ $string == abc* ]] and [[ $string == "abc*" ]]
[[ $string == abc* ]] - will check if string begins with abc letters
[[ $string == "abc*" ]] - will check if string is equal exactly to abc*
57) How to list usernames which starts with ab or xy ?
egrep "^ab|^xy" /etc/passwd|cut -d: -f1

58) What $! means in bash ?
Most recent background command PID

59) What $? means ?
Most recent foreground exit status.

60) How to print PID of the current shell ?
echo $$

61) How to get number of passed arguments to the script ?
echo $#

62) What difference between $* and $@
$* - gives all passed arguments to the script as a single string
$@ - gives all passed arguments to the script as delimited list. Delimiter $IFS

63) How to define array in bash ?
array=("Hi" "my" "name" "is")

64) How to print the first array element ?
echo ${array[0]}

65) How to print all array elements ?
echo ${array[@]}

66) How to print all array indexes ?
echo ${!array[@]}

67) How to remove array element with id 2 ?
unset array[2]

68) How to add new array element with id 333 ?
array[333]="New_element"

69) How shell script get input values ?
a) via parameters

./script param1 param2

b) via read command

read -p "Destination backup Server : " desthost

70) How can we use "expect" command in a script ?
/usr/bin/expect << EOD
spawn rsync -ar ${line} ${desthost}:${destpath}
expect "*?assword:*"
send "${password}\r"
expect eof
EOD

Good luck !! Please comment below if you have any new query or need answers for your questions. Let us know how well this helped for your interview :-)

---------------------------------------------------------------------------------------------------------

1)to sum
((sum=25+35))
echo $sum

2)diff
((diff=35-10))
echo $diff

3)multi
((mult=35*10))
echo $mult

4)div
((div=40/10))
echo $div

5)The value of count variable will increment by 1 in each step. 
When the value of count variable will 5 then the while loop will terminate.

valid=true
count=1
while [ $valid ]
do
echo $count
if [ $count -eq 5 ];
then
break
fi
((count++))
done

6)area
((area=5*5))
echo $area

7.for------- loop
#!/bin/bash
for (( counter=10; counter>0; counter-- ))
do
echo -n "$counter "
done
printf "\n"

8.Finding odd and even number using three expressions
for (( n=1; n<=5; n++ ))
do
if (( $n%2==0 ))
then
echo "$n is even"
else
echo "$n is odd"
fi
done

9.Reading static values

for color in Blue Green Pink White Red
do
echo "Color = $color"
done

10.http://www.freeos.com/guides/lsst/ch08.html#q4


11.#!/bin/bash
# declare integers
NUM1=2
NUM2=1
if   [ $NUM1 -eq $NUM2 ]; then
	echo "Both Values are equal"
elif [ $NUM1 -gt $NUM2 ]; then
	echo "NUM1 is greater then NUM2"
else 
	echo "NUM2 is greater then NUM1"
fi 


12.Problem: I have a file called 'mypaper.txt' or 'mypaper.tex' or some such somewhere, but I don't remember where I put it. How do I find it?

Solution:

$ find ~ -name "mypaper*" 


13.Problem: I have a file containing a list of words. I want to sort it in reverse order and print it.

Solution:

$ cat myfile.txt | sort -r | lpr



14.Problem: my data file has some repeated lines! How do I get rid of them?

Solution:

$ sort datafile.dat | uniq > newfile.dat



15.Problem: I have a text file containing the word 'entropy' in this directory, is there anything like SEARCH?

Solution: yes, try

$ grep -l 'entropy' *


16.
-----------------------------------------------------------------------------------------------------------------------------------

how to create ec2 instance with terraform


vi instance.tf
change access key and secret key and save it
then terraform init
then terraform apply
then terraform show
cd ..
cd demo-1
it will show
provider.tf
instance.tf
vars.tf
 we need to create a file so for that we open cat vars.tf
we need to edit (vi terraform.tfvars)

AWS_ACCESS_KEY="AKIAJ4EZGWG2HDVC3KHQ"
AWS_SECRET_KEY="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"
AWS_REGION="us-east-1"

then cat terraform.tfvars(to see)

--->cat provider.tf
----->cat vars.tf
---->terraform init
---->terraform apply

------->open cd demo-2
in this 
cp ../demo-1/terraform.tfvars .
cat vars.tf
cat provider.tf
cat script.sh
vi script.sh------> change this sleep time 5 
cat instance.t
terraform init
then we need to generate key
ssh-keygen.exe -f mykey
cp mykey mykey.pem
terraform.exe apply
to login instance do this
chmod 400 mykey.pem
 ssh -i mykey.pem ubuntu@172.31.82.116

-------->packer.........
cd packer-demo
ls-ltr
cat build-and-launch.sh
cat packer-example.json
ls -ltrh
cat key.tf
cp ../demo-2/mykey* .
ls -ltrh
cp ../demo-2/terraform.tfvars .
./build-and-launch.sh
 vi packer-example.json
enter access and secret keys
./build-and-launch.sh




---------------vedios----------------------------

https://s3.amazonaws.com/aws-videos1/spinnaker_setup.wrf
https://s3.amazonaws.com/aws-videos1/log_analysis.wrf
https://s3.amazonaws.com/aws-videos1/pkr_tf.wrf

------------------------------------------------------------


