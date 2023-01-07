# Day 5: Few more concepts on Shell Scripting

Hi Everyone!

Please find below concepts on shell scripting. (continuation)

### Shift Command

Generally, arguments will take from $1 to $9. ($0 gives filename).

if we give $10, it won't consider the 10th argument instead go for the first argument.

So you can give them {} braces

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672993799982/aa24918a-3d96-4f39-be86-0217c789618b.png align="center")

or use shift command as explained below.

By using the shift command it is possible to reference individual CLI parameters beyond $9.

Shift command move to one argument ahead.

Ex: If we give a, b, c arguments, it won't consider the first argument but move to the second one.

shift -- move/points to the second argument (If you don't mention any number after shift, it considers as '1')

shift 2 -- move to the third argument

shift 3 -- move to the fourth argument and so on.

Note: So in order to consider the first argument make sure you either use this first value (or)

Store in another variable like first = $1

Otherwise, it will be lost.

**An Example script:**

Here $@ holds all the arguments. If we shift 3 then it will consider from 4th argument and prints four , five , six

```bash
#!/bin/bash
echo "Input: $@"
shift 3
echo "After shift: $@"
```

Run it:

```bash
$ myscript.sh one two three four five six

Input: one two three four five six
After shift: four five six
```

Shift in while

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672989995880/3ff117d1-43ba-4065-a94e-a78773835358.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672992960368/42466596-b83d-40b8-a4e6-121a79827181.png align="center")

---

### **Types of Operators**

Arithmetic Operations:

In the shell script, all variables hold string values even if they are numbers. So, to perform arithmetic operations we use the `expr` command.

The `expr` command can only work with integer values. For floating point numbers we use the `bc` command.

To compute the result we enclose the expression in backticks

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672911615679/2d460970-d0bf-4e67-9bc4-6ad564682177.png align="center")

**Examples: For Addition**

#!/bin/sh

take two integers from the user

echo "Enter two integers: " read a b

perform addition

result=**expr $a + $b**

show result

echo "Result: $result"

---

**Floating:**

In the following example we are enclosing the variables in double quotes and using bc to handle floating point numbers.

#!/bin/sh

take two numbers from the user

echo "Enter two numbers: " read a b

perform addition

result=**expr "$a + $b" | bc**

show result

echo "Result: $result"

---

**Relational Operators:**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672933696974/5a8f3eb2-993f-4aac-ac2d-9940c72913d4.png align="center")

if(( $a==$b ))

then

echo "a is equal to b."

else

echo "a is not equal to b."

fi

---

**Boolean Operators:**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672934798030/72f9bda2-374e-4673-8ebf-24ae3b6b6280.png align="center")

---

**String Operators:**

[String Operators | Shell Script - GeeksforGeeks](https://www.geeksforgeeks.org/string-operators-shell-script/)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672936108854/9610ddd8-4581-42d6-8fab-bcc8332c07f7.png align="center")

**File Test operators:**

[Unix / Linux - Shell File Test Operators Example (](https://www.tutorialspoint.com/unix/unix-file-operators.htm#:~:text=Unix%20Command%20Course%20for%20Beginners%20%20%20,%5D%20is%20true.%20%2010%20more%20rows%20)[tutorialspoint.com](http://tutorialspoint.com)[)](https://www.tutorialspoint.com/unix/unix-file-operators.htm#:~:text=Unix%20Command%20Course%20for%20Beginners%20%20%20,%5D%20is%20true.%20%2010%20more%20rows%20)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1672936943023/8f18fd25-f011-463f-81eb-d14fd21916f7.png align="center")

---

### **Functions**

[Unix / Linux - Shell Functions (](https://www.tutorialspoint.com/unix/unix-shell-functions.htm#:~:text=Unix%20%2F%20Linux%20-%20Shell%20Functions%201%20Creating,Functions%20...%205%20Function%20Call%20from%20Prompt%20)[tutorialspoint.com](http://tutorialspoint.com)[)](https://www.tutorialspoint.com/unix/unix-shell-functions.htm#:~:text=Unix%20%2F%20Linux%20-%20Shell%20Functions%201%20Creating,Functions%20...%205%20Function%20Call%20from%20Prompt%20)

**Creating Functions:**

functionname () {

}

```plaintext
#!/bin/sh

# Define your function here
Hello () {
   echo "Hello Aasifa"
}

# Invoke your function
Hello
```

```plaintext
$./test.sh
Hello Aasifa
```

**Passing Parameters to the Functions:**

```plaintext
#!/bin/sh

# Define your function here
Hello () {
   echo "Hello World $1 $2"
}

# Invoke your function
Hello Aasifa Shaik
```

```plaintext
$./test.sh
Hello World Aasifa Shaik
```

**Returning Values from the functions:**

`$?` returns the exit value of the last executed command.

```plaintext
#!/bin/sh

# Define your function here
Hello () {
   #echo "Hello World $1 $2"
   return 10
}

# Invoke your function
Hello Aasifa shaik

# Capture value returnd by last command
ret=$?

echo "Return value is $ret"
```

```plaintext
Return value is 10
```

**Nested Functions:**

```plaintext
#!/bin/sh

# Calling one function from another
number_one () {
   echo "This is the first function speaking..."
   number_two
}

number_two () {
   echo "This is now the second function speaking..."
}

# Calling function one.
number_one
```

```plaintext
This is the first function speaking...
This is now the second function speaking...
```