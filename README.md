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
