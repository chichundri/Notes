#How to convert decimal number to binary
static void usingArray() {
	//initialize array
	int binary[] = new int[20];
        //declare the decimal number to be converted to binary
	int decimalNumber = 25;
	int index = 0;
	//loop till the number is greater than 0
	while (decimalNumber > 0) {
		//divide the number by 2 using modulus operator so that we get the remainder
		int remainder = decimalNumber % 2;
		//store the remainder in array
		binary[index++] = remainder;
		//divide the number to get the quotient and assign it back to the number
		decimalNumber = decimalNumber / 2;
	}
	//loop over the array backwards and print the binary number
	for (int i = index - 1; i &gt;= 0; i--) {
		System.out.print(binary[i]);
	}
}

static void usingWrapperClass() {
	// declare the decimal number to be converted to binary
	int decimalNumber = 25;
	//convert the decimal number to binary using Integer class method
	String binaryNumber = Integer.toBinaryString(decimalNumber);
	System.out.println(binaryNumber);
}

# How to divide two numbers without using division (/) operator in java / How to find remainder of division of two numbers using minus (-) operator / Java program to find remainder of division using – 
    ans - subtrct divisor from dividend 
    while(dividend >= divisor)
   {
      dividend = dividend - divisor;
      quotient++;
   }
#How to convert each alternate character of String to upper case in java
public class CharacterConverter {

   public static void main(String[] args) {
	// initialize string
	String sample = "codippa";
	// initialize string buffer to hold updated string
	StringBuffer updatedString = new StringBuffer();
	// get array of characters in string
	char[] characters = sample.toCharArray();
	// iterate over characters
	for (int index = 0; index < characters.length; index++) {
		// get current character
		char c = characters[index];
		// check if position of this character is odd
		if (index % 2 != 0) {
			// convert it to upper case
			c = Character.toUpperCase(c);
		}
		// append character to string buffer
		updatedString.append(c);
	}
	System.out.println("Modified string is: " + updatedString.toString());
   }
}

#How to count the number of objects of a class in java

#Program to remove duplicate characters from a word in Java / How to remove duplicate characters from a word in Java
static void method2(){
	Set set = new LinkedHashSet();
	Scanner scanner = new Scanner(System.in);
	System.out.println("Please enter a word");
	String word = scanner.next();
	for (int i = 0; i < word.length(); i++) {
		set.add(word.charAt(i));
	}
	StringBuffer buffer = new StringBuffer();
	for (Iterator iterator = set.iterator(); iterator.hasNext();) {
		Character character = (Character) iterator.next();
		buffer.append(character);
	}
	System.out.println(buffer);
        scanner.close()
}

#How to calculate GCD in java / Various ways of GCD calculation in java / Java program to calculate GCD
static int calculateGcdByRepeatedDivision(int numberOne, int numberTwo) {
        //iterate a loop while remainder is not zero
	while (numberTwo != 0) {
                //get the remainder of division
		int remainder = numberOne % numberTwo;
                // make the divisor of this step as the dividend of next step
		numberOne = numberTwo;
                // assign remainder to this variable since this is the divisor 
                //for the next step and it is also the condition of loop
		numberTwo = remainder;
	}
	return numberOne;
   }







