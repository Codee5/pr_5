###**Задачи на языке программирования Go Пашенин А.М. ЭФМО-01-24**

**Линейное программирование**
```cpp
 №1
package main

import (
 "fmt"
 "math"
 "strings"
)

// Функция для перевода числа из заданной системы счисления в десятичную
func toDecimal(number string, base int) (int64, error) {
 numLen := len(number)
 var decimalValue int64 = 0

 for i, char := range number {
  var value int64
  if char >= '0' && char <= '9' {
   value = int64(char - '0') // Для цифр 0-9
  } else if char >= 'A' && char <= 'Z' {
   value = int64(char-'A') + 10 // Для букв A-Z
  } else if char >= 'a' && char <= 'z' {
   value = int64(char-'a') + 10 // Для букв a-z
  } else {
   return 0, fmt.Errorf("недопустимый символ: %c", char)
  }

  if value >= int64(base) {
   return 0, fmt.Errorf("недопустимый символ для базы %d: %c", base, char)
  }

  decimalValue += value * int64(math.Pow(float64(base), float64(numLen-i-1)))
 }

 return decimalValue, nil
}

// Функция для перевода из десятичной системы в заданную систему счисления
func fromDecimal(decimalValue int64, base int) string {
 if decimalValue == 0 {
  return "0"
 }

 var result strings.Builder
 for decimalValue > 0 {
  remainder := decimalValue % int64(base)
  if remainder < 10 {
   result.WriteByte(byte(remainder + '0'))
  } else {
   result.WriteByte(byte(remainder-10+'A'))
  }
  decimalValue /= int64(base)
 }

 // Переворачиваем строку, так как мы добавляли цифры в обратном порядке
 return reverse(result.String())
}

// Функция для переворота строки
func reverse(s string) string {
 r := []rune(s)
 for i, j := 0, len(r)-1; i < j; i, j = i+1, j-1 {
  r[i], r[j] = r[j], r[i]
 }
 return string(r)
}

func main() {
 var number string
 var inputBase, outputBase int

 fmt.Print("Введите число: ")
 fmt.Scan(&number)
 fmt.Print("Введите исходную систему счисления (2-36): ")
 fmt.Scan(&inputBase)
 fmt.Print("Введите конечную систему счисления (2-36): ")
 fmt.Scan(&outputBase)

 // Проверка на корректность баз
 if inputBase < 2 || inputBase > 36 || outputBase < 2 || outputBase > 36 {
  fmt.Println("Система счисления должна быть в диапазоне от 2 до 36")
  return
 }

 // Переводим число в десятичную систему
 decimalValue, err := toDecimal(number, inputBase)
 if err != nil {
  fmt.Println("Ошибка:", err)
  return
 }

 // Переводим из десятичной системы в конечную
 result := fromDecimal(decimalValue, outputBase)
 fmt.Printf("Число %s в системе счисления %d переводится в: %s в системе счисления %d\n", number, inputBase, result, outputBase)
}
№2
package main

import (
	"fmt"
	"math"
)

var a, b, c float64

func main() {
	fmt.Println("Введите коэффициент a:")
	fmt.Scanln(&a)

	fmt.Println("Введите коэффициент b:")
	fmt.Scanln(&b)

	fmt.Println("Введите коэффициент c:")
	fmt.Scanln(&c)

	discriminant := b*b - 4*a*c

	if discriminant > 0 {
		root1 := ((-1*b) + math.Sqrt(discriminant)) / (2 * a)
		root2 := ((-1*b) - math.Sqrt(discriminant)) / (2 * a)
		fmt.Printf("Два корня: %.2f и %.2f\n", root1, root2)
	} else if discriminant == 0 {
		root := (-1*b) / (2 * a)
		fmt.Printf("Один корень: %.2f\n", root)
	} else {
		realPart := -1*b / (2 * a)
		imaginaryPart := math.Sqrt(-discriminant) / (2 * a)
		fmt.Printf("Комплексные корни: %.2f + %.2fi и %.2f - %.2fi\n", realPart, imaginaryPart, realPart, imaginaryPart)
	}
}

№3
package main

import (
	"fmt"
)

var a [6]int

func main() {
	for i := 0; i <= 5; i++ {
		var inp int
		fmt.Scan(&inp)
		a[i] = inp
	}
	for i := 0; i <= 5; i++ {
		for j := i; j < 5; j++ {
			if a[i] > a[j+1] {
				var c = 0
				c = a[j+1]
				a[j+1] = a[i]
				a[i] = c
			}
		}
	}
	fmt.Println(a)
}

№4

package main

import (
	"fmt"
)

var a [6]int
var b [6]int
var ab [12]int

func main() {
	for i := 0; i <= 5; i++ {
		var inp int
		fmt.Scan(&inp)
		a[i] = inp
	}
	for i := 0; i <= 5; i++ {
		for j := i; j < 5; j++ {
			if a[i] > a[j+1] {
				var c = 0
				c = a[j+1]
				a[j+1] = a[i]
				a[i] = c
			}
		}
	}

	for i := 0; i <= 5; i++ {
		var inp int
		fmt.Scan(&inp)
		b[i] = inp
	}
	for i := 0; i <= 5; i++ {
		for j := i; j < 5; j++ {
			if b[i] > b[j+1] {
				var c = 0
				c = b[j+1]
				b[j+1] = b[i]
				b[i] = c
			}
		}
	}
	for i := 0; i <= len(a)-1; i++ {
		ab[i] = a[i]
	}
	var j int = len(a)
	for i := 0; i <= len(b)-1; i++ {
		ab[j] = b[i]
		j = j + 1
	}
	for i := 0; i <= 11; i++ {
		for j := i; j < 11; j++ {
			if ab[i] > ab[j+1] {
				var c = 0
				c = ab[j+1]
				ab[j+1] = ab[i]
				ab[i] = c
			}
		}
	}
	fmt.Println(ab)
}

№5

package main

import (
	"fmt"
)

var a string
var b string

func main() {
	fmt.Println("Введите основную строку")
	fmt.Scanln(&a)
	fmt.Println("Введите символы для поиска")
	fmt.Scanln(&b)

	var j int = 0
	var ind int = 0
	var i int = 0
	for i = 0; i < len(a)-1; i++ {
		if a[i] == b[j] {
			j = j + 1
			if j == 1 {
				ind = i
			}
			if j == len(b) {
				fmt.Println(ind)
				break
			}
		} else {
			j = 0
		}
	}
	if j < len(b)-1 {
		fmt.Println("-1")
	}
}
```

 **Условия**
 ```cpp
№1

package main

import (
	"fmt"
)

var a int
var b int
var c string
var res int

func main() {

	fmt.Println("Введите значение 1:")
	fmt.Scan(&a)
	fmt.Println("Введите значение 2:")
	fmt.Scan(&b)
	fmt.Println("Введите символ действия:")
	fmt.Scan(&c)

	switch c {
	case "+":
		res = a + b
		fmt.Println(res)
	case "-":
		res = a - b
		fmt.Println(res)
	case "*":
		res = a * b
		fmt.Println(res)
	case "/":
		if b == 0 {
			fmt.Println("На ноль делить нельзя!!!")
		} else {
			res = a / b
			fmt.Println(res)
		}
	case "%":
		if b == 0 {
			fmt.Println("На ноль делить нельзя!!!")
		} else {
			res = a % b
			fmt.Println(res)
		}
	default:
		fmt.Println("Вы ввели недопустимый символ")
	}
}

№2

package main

import (
	"fmt"
	"unicode"
)

func isPalindrome(input string) bool {
	
	var cleaned []rune
	for _, char := range input {
		if unicode.IsLetter(char) || unicode.IsDigit(char) {
			cleaned = append(cleaned, unicode.ToLower(char))
		}
	}
	n := len(cleaned)
	for i := 0; i < n/2; i++ {
		if cleaned[i] != cleaned[n-1-i] {
			return false
		}
	}

	return true
}

func main() {
	var input string

	fmt.Print("Введите строку: ")
	fmt.Scanln(&input)

	if isPalindrome(input) {
		fmt.Println("Строка является палиндромом.")
	} else {
		fmt.Println("Строка не является палиндромом.")
	}
}

№3

package main

import (
	"fmt"
)

var a1 int
var a2 int
var b1 int
var b2 int
var c1 int
var c2 int

func main() {

	fmt.Println("Введите отрезок 1:")
	fmt.Scan(&a1)
	fmt.Scan(&a2)
	fmt.Println("Введите отрезок 2:")
	fmt.Scan(&b1)
	fmt.Scan(&b2)
	fmt.Println("Введите отрезок 3:")
	fmt.Scan(&c1)
	fmt.Scan(&c2)

	if a2 >= b1 && a2 >= c1 && a1 <= b2 && a1 <= c2 {
		fmt.Println(true)
	} else if b2 >= a1 && b2 >= c1 && b1 <= a2 && b1 <= c2 {
		fmt.Println(true)
	} else if c2 >= b1 && c2 >= a1 && c1 <= b2 && c1 <= a2 {
		fmt.Println(true)
	} else {
		fmt.Println(false)
	}
}
№4

package main

import (
	"fmt"
	"regexp"
	"strings"
)

func findLongestWord(sentence string) string {
	re := regexp.MustCompile(`[^\w\s]+`)
	cleaned := re.ReplaceAllString(sentence, "")
	words := strings.Fields(cleaned)
	longestWord := ""
	for _, word := range words {
		if len(word) > len(longestWord) {
			longestWord = word
		}
	}

	return longestWord
}

func main() {
	var input string
	fmt.Print("Введите предложение: ")
	fmt.Scanln(&input)
	longestWord := findLongestWord(input)
	if longestWord != "" {
		fmt.Println("Самое длинное слово:", longestWord)
	} else {
		fmt.Println("Не удалось найти слова в предложении.")
	}
}

№5

ackage main

import (
	"fmt"
)

var year int
var a, b, c int

func main() {

	fmt.Println("Введите год:")
	fmt.Scan(&year)

	a = year % 4
	b = year % 100
	c = year % 400
	if a == 0 && b != 0 {
		fmt.Print(true)
	} else if c == 0 {
		fmt.Print(true)
	} else {
		fmt.Print(false)
	}

}
```
**Циклы**
 ```cpp

№1

package main

import (
	"fmt"
)

var limit int

func main() {

	fmt.Print("Введите верхнюю границу для чисел Фибоначчи: ")
	fmt.Scan(&limit)

	a, b := 0, 1

	for a <= limit {
		fmt.Println(a)
		a, b = b, a+b
	}
}


№2

package main

import (
	"fmt"
)

var a, b int

func main() {

	fmt.Println("Введите числа:")
	fmt.Scan(&a, &b)

	for a = a + 1; a <= b-1; a++ {
		fmt.Println(a)
	}
}

№3

package main

import (
	"fmt"
	"math"
)
func isArmstrong(number int) bool {
	digits := make([]int, 0)
	n := number
	for n > 0 {
		digit := n % 10
		digits = append(digits, digit)
		n /= 10
	}
	numDigits := len(digits)
	sum := 0
	for _, digit := range digits {
		sum += int(math.Pow(float64(digit), float64(numDigits)))
	}
	return sum == number
}

func main() {
	var lower, upper int
	fmt.Print("Введите нижнюю границу диапазона: ")
	fmt.Scan(&lower)
	fmt.Print("Введите верхнюю границу диапазона: ")
	fmt.Scan(&upper)
	fmt.Printf("Числа Армстронга в диапазоне от %d до %d:\n", lower, upper)
	for i := lower; i <= upper; i++ {
		if isArmstrong(i) {
			fmt.Println(i)
		}
	}
}

№4

package main

import (
	"fmt"
)

func reverseString(s string) string {

	reversed := make([]byte, len(s))

	for i := 0; i < len(s); i++ {
		reversed[len(s)-1-i] = s[i]
	}

	return string(reversed)
}

func main() {
	var input string

	fmt.Print("Введите строку: ")
	fmt.Scanln(&input)

	reversedString := reverseString(input)

	fmt.Printf("Обратная строка: %s\n", reversedString)
}

№5

package main

import (
	"fmt"
)
func gcd(a, b int) int {
	for b != 0 {
		temp := b
		b = a % b
		a = temp
	}
	return a
}

func main() {
	var num1, num2 int
	fmt.Print("Введите первое число: ")
	fmt.Scan(&num1)
	fmt.Print("Введите второе число: ")
	fmt.Scan(&num2)
	result := gcd(num1, num2)
	fmt.Printf("Наибольший общий делитель чисел %d и %d равен %d\n", num1, num2, result)
}
```
