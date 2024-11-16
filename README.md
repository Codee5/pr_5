**Линейное программирование**
```cpp
 №1 package main

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
```
