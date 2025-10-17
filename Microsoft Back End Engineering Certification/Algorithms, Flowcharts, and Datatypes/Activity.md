
Problem 1: Discount Eligibility
Start
	Prompt user with "Input Total Spending" as TotalSpending
	Make sure the input is a positive number.
	If the customer has spent 100 or more:
		Print 10% discount applied
	Otherwise:
		No discount
End


Problem 2: Book Categorisation

Start
	Prompt user with "input Genre of book" as Genre
	If Genre is "Fiction":
		Print "Category: Fiction"
	Otherwise if Genre is "Non-Fiction":
		Print "Category: Non-Fiction"
	Otherwise if Genre is "Science Fiction":
		Print "Category: Science Fiction"
	Otherwise:
		Print "Category: Unknown"
End

Problem 3: Even or Odd Number

Start
	Prompt user with "input number" as num
	Make sure the number is an integer
	if num % 2 is 0:
		Print "Even number"
	Otherwise:
		Print "Odd number"

End




## Problem 1: Calculating the Sum of an Array

```C#
int sum = 0;
int[] num = {1, 2, 3, 4, 5};

for (int i = 0; i < num.Length(); i++)
{
	sum += num[i];
}

Console.WriteLine(sum);
```

## Problem 2: Counting the Number of Vowels in a String


```C#
int vowelCount = 0;
char vowels = {'a', 'e', 'i', 'o', 'u'};
string givenString = "I love Programming, and so should you!"

foreach (char c in givenString)
{
	foreach (char ch in vowels)
	{
		vowelCount += 1;
	}
}

Console.WriteLine(vowelCount);

```