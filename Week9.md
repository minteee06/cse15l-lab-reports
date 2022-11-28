Part 1: My grading script

rm -rf student-submission

echo "Removed student-submission directory"
git clone $1 student-submission
echo "cloned repository"

cp TestListExamples.java student-submission
echo "Copied file into student-submission"

cp -r lib student-submission
echo "Copied lib directory into student-submission"

cd student-submission
echo "Changed directory to student-submission"

FILE=ListExamples.java
if [ ! -f "$FILE" ];
then
echo "$FILE was not found"
exit
fi

javac -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar \*.java 2> errorResult.txt
if [ -s errorResult.txt ];
then
echo "Compile Error"
exit
fi

echo "Compiled success"

java -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar org.junit.runner.JUnitCore TestListExamples > result.txt

echo "Tests passed"

if grep -q "2 tests" "result.txt" ;
then
echo "Congratulations. You passed!"
exit
fi

cat result.txt
echo "Test failed. Try again!"
