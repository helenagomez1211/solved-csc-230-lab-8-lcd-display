Download Link: https://assignmentchef.com/product/solved-csc-230-lab-8-lcd-display
<br>
During previous labs, when we worked with c-strings, we had to use the Atmel Studio debugger and manually examine the memory in order to see the string. It would be much easier if we could display the string on the Liquid Crystal Display (LCD). The one that we use (HD44780) has its own, built-in controller and driver chips and it is capable of displaying some ASCII text characters and some special symbols. Our LCD display consists of two rows, each can display up to 16 characters.

Let’s see how we can interface with the LCD. Create a new project and download the two LCD

Driver files into the newly created project directory, which already contains the main.asm file: <em>LCDdefs.inc</em> (LCD driver) <em>lcd.asm</em>  (LCD driver)

Download <em>lab8.asm</em> and <em>numDisplay.asm</em> into the same place as above. The directory of your project should look like this:

In the <em>Solution Explorer</em> of the <em>Atmel Studio 7.0</em> project, add <em>lab8.asm</em> and <em>numDisplay.asm</em> to your project and set <em>lab8.asm</em> as entry file. The project Solution Explorer will then look like this:

Build the project. In <em>Solution Explorer</em> of the <em>Atmel Studio 7.0</em> project, expand the <em>Dependencies</em> folder. The <em>Solution Explorer</em> should look like this:

The following diagram demonstrates the relationships between the program (Flash) memory, data

(SRAM) memory, the AVR CPU of the AVR board and the LCD display. In lab8.asm, the two strings “Line1” and “Line2” are copied from the program memory to the data memory. Then the two strings are displayed from the data memory to the LCD display. Notice that the program memory is word (2 bytes per word) addressable and the data memory is byte addressable.

Note that <em>str_init()</em> function is defined in <em>lcd.asm</em>. It copies a string from the program memory (flash) to the data memory (SRAM), like what we did in previous labs and assignments. “SP_OFFSET” is defined in LCDdefs.inc (line 42 and 43). It is the number of bytes of the return address for a subroutine call, which is 3 bytes in the case of ATMEGA 2560.

<strong>Open lab8.asm. Read and understand the codes</strong>

In the <em>display_strings</em> subroutine, the algorithm is doing the following:

Clear the LCD display (<em>lcd_clr</em>)

Move the cursor to the desired location (row 0, column 0) (2X16 display) (<em>lcd_gotoxy</em>)     Display “Line1” stored in SDRAM (<em>lcd_puts</em>)

Move the cursor to the desired location (row 1, column 0) (2X16 display) (<em>lcd_gotoxy</em>)

Display “Line2” stored in SDRAM (<em>lcd_puts</em>)

<ol>

 <li><strong> Exercises: </strong></li>

 <li>Set <em>asm</em> as entry file. Finish implementing the main program to display the number. Modify the subroutine <em>void int_to_string() </em>such that it accepts two parameters passed on the stack, which are the number to be displayed and the address where to store the string. The string is to be stored in c-string format (zero-terminated). It is up to you whether to make the string left-justified or right-justified within the allocated space. The function can assume that enough space is available at the address that is passed to it. If time permits, modify the function <em>int_to_string()</em> to accept a 24-bit number and which will have 8 digits in decimal notation.</li>

</ol>




The C equivalent code is available in <em>divide.c</em> file:




<table width="0">

 <tbody>

  <tr>

   <td width="619">#include &lt;stdio.h&gt;/*division, using repeated subtractions.Convert integer 123 to a character array: ‘1’ ‘2’ ‘3’ ‘ ’The last character ‘ ’ indicates the end of the character array.*/ int main() {int dividend = 1234681;             int quotient = 0;          int divisor = 10;  char num_in_char_array[4];             num_in_char_array[3] = ‘ ’;int i = 2;do {while(dividend &gt;= divisor) {quotient++;dividend -= divisor;}num_in_char_array[i–] = dividend + ‘0’;dividend = quotient;                quotient = 0;} while(dividend &gt;= divisor);             num_in_char_array[i] = dividend + ‘0’;printf(“%s
”, num_in_char_array);  return 0;}</td>

  </tr>

 </tbody>

</table>




You may download <em>divide.c</em>, compile and run the code from the DOS command window:

<ol start="2">

 <li>Detect which button on the LCD shield is pressed and display its name on the LCD; the exact location is your choice. If no button is pressed, display a string containing all spaces in place of the button name (to erase the name). For this exercise, feel free to use your modified version of the check_button function from the previous lab to detect which button is pressed.</li>

 <li>Count the number of individual button presses and display it on the LCD; the exact location is your choice. Your code should ignore the button while it remains being pressed down and count only the individual presses in-between the button releases. You can use the newly modified int_to_string assembly function for this exercise.</li>

</ol>