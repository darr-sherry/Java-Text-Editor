# Java-Text-Editor
import java.util.Stack;
import java.util.*;
class Buffer
{
 private Stack<Character> bufferLeft;
private Stack<Character> bufferRight;
// necessary when serializing buffer
private boolean reverse = true;
// create an empty buffer
public Buffer()
{
bufferLeft = new Stack<Character>();
bufferRight = new Stack<Character>();
}
// insert c at the cursor position
public void insert(char c)
{
bufferLeft.push(c);
}
// delete and return the character at the cursor
public char delete() 
{
return bufferRight.isEmpty() ? '\0' : bufferRight.pop();
}
// move the cursor k positions to the left
public void left(int k)
{
while(!bufferLeft.isEmpty() && --k >= 0) 
bufferRight.push(bufferLeft.pop());
}
// move the cursor k positions to the right
public void right(int k)
{
while(!bufferRight.isEmpty() && --k >= 0) 
bufferLeft.push(bufferRight.pop());
}
// number of characters in the buffer
public int size()
{
return bufferLeft.size() + bufferRight.size();
}
// class-specific helper function to serialize each buffer
private String serializeBuffer(Stack<Character> bf)
{
int size = bf.size();
StringBuilder out = new StringBuilder();
if(reverse = !reverse)
{
for(int i = size - 1; i>=0; --i)
{
out.append(bf.get(i));
}
} 
else 
{
for(int i = 0; i < size; ++i)
{
out.append(bf.get(i));
}
}
return out.toString();
}
public String toString()
{
return serializeBuffer(bufferLeft) + 
serializeBuffer(bufferRight);
}
}
public class TextEditor
{
 public static void main(String args[])
{
Buffer textEditor = new Buffer();
Scanner cmd = new Scanner(System.in);
// Print instructions
System.out.println("This is a simple text editor.\n\n"
+ "\t- '>' represents the location of the 
cursor.\n"
+ "\t- Type any character and press enter to 
add it to the stream.\n"
+ "\nThe following is a list of commands. "
+ "\n\n\t+C--->Add a special character (C) to 
the stream."
+ "\n\t?---->Get information about the stream 
(i.e., the size)."
+ "\n\t*---->Quit the text editor"
+ "\n\t<#--->Move the cursor left by (#) 
number of places."
+ "\n\t>#--->Move the cursor right by (#) 
number of places.\n\n");
while(true)
{
System.out.println("\t" + textEditor);
System.out.print(" > ");
String command = cmd.nextLine();
char query = command.isEmpty() ? '\0' : 
command.charAt(0);
boolean negative = false;
switch(query)
{
case '-':
textEditor.delete();
break;
case '?':
System.out.println("Number of characters: " + 
textEditor.size());
break;
case '*':
System.out.println("Goodbye!");
cmd.close();
return;
case '<':
negative = true;
case '>':
int arg = new Integer(command.substring(1, 
command.length()));
if(negative)
textEditor.left(arg);
else
textEditor.right(arg);
break;
case '\0':
break;
case '+':
query = command.charAt(1);
default:
textEditor.insert(query);
break;
}
}
}
}
