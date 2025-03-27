# interface


 Задача 1: Подсчет символов и простых множителей числа

```cpp
#include <iostream>
#include <string>
#include <cmath>
#include <set>

class ICount {
public:
    virtual int count(int number) = 0; // Чистая виртуальная функция
};

class DigitCount : public ICount {
public:
    int count(int number) override {
        return std::to_string(std::abs(number)).length();
    }
};

class PrimeFactorCount : public ICount {
public:
    int count(int number) override {
        std::set<int> primeFactors;
        for (int i = 2; i <= number; i++) {
            while (number % i == 0) {
                primeFactors.insert(i);
                number /= i;
            }
        }
        return primeFactors.size();
    }
};

int main() {
    int number;
    std::cout << "Введите число: ";
    std::cin >> number;

    DigitCount digitCounter;
    PrimeFactorCount primeCounter;

    std::cout << "Количество символов: " << digitCounter.count(number) << std::endl;
    std::cout << "Количество различных простых множителей: " << primeCounter.count(number) << std::endl;

    return 0;
}
```

 Задача 2: Сравнение чисел

```cpp
class IMatch {
public:
    virtual bool match(int number1, int number2) = 0; // Чистая виртуальная функция
};

class GreaterThan : public IMatch {
public:
    bool match(int number1, int number2) override {
        return number1 > number2;
    }
};

class CoprimeCheck : public IMatch {
private:
    int gcd(int a, int b) {
        while (b != 0) {
            int temp = b;
            b = a % b;
            a = temp;
        }
        return a;
    }
public:
    bool match(int number1, int number2) override {
        return gcd(number1, number2) == 1; // Они взаимно простые, если GCD = 1
    }
};

int main() {
    int num1, num2;
    std::cout << "Введите первое число: ";
    std::cin >> num1;
    std::cout << "Введите второе число: ";
    std::cin >> num2;

    GreaterThan greaterChecker;
    CoprimeCheck coprimeChecker;

    std::cout << "Первое число больше второго: " << (greaterChecker.match(num1, num2) ? "true" : "false") << std::endl;
    std::cout << "Числа взаимно простые: " << (coprimeChecker.match(num1, num2) ? "true" : "false") << std::endl;

    return 0;
}
```

 Задача 3: Сумма и произведение элементов массива

```cpp
class IAccumulator {
public:
    virtual int fold(int arr[], int size) = 0; // Чистая виртуальная функция
};

class SumAccumulator : public IAccumulator {
public:
    int fold(int arr[], int size) override {
        int sum = 0;
        for (int i = 0; i < size; i++) {
            sum += arr[i];
        }
        return sum;
    }
};

class ProductAccumulator : public IAccumulator {
public:
    int fold(int arr[], int size) override {
        int product = 1;
        for (int i = 0; i < size; i++) {
            product *= arr[i];
        }
        return product;
    }
};

int main() {
    int size;
    std::cout << "Введите размер массива: ";
    std::cin >> size;
    int* arr = new int[size];

    std::cout << "Введите элементы массива: ";
    for (int i = 0; i < size; i++) {
        std::cin >> arr[i];
    }

    SumAccumulator sumAccumulator;
    ProductAccumulator productAccumulator;

    std::cout << "Сумма элементов: " << sumAccumulator.fold(arr, size) << std::endl;
    std::cout << "Произведение элементов: " << productAccumulator.fold(arr, size) << std::endl;

    delete[] arr; // Освобождение памяти
    return 0;
}
```

 Задача 4: Подсчет четных и нечетных чисел

```cpp
class EvenCountAccumulator : public IAccumulator {
public:
    int fold(int arr[], int size) override {
        int count = 0;
        for (int i = 0; i < size; i++) {
            if (arr[i] % 2 == 0) {
                count++;
            }
        }
        return count;
    }
};

class OddCountAccumulator : public IAccumulator {
public:
    int fold(int arr[], int size) override {
        int count = 0;
        for (int i = 0; i < size; i++) {
            if (arr[i] % 2 != 0) {
                count++;
            }
        }
        return count;
    }
};

int main() {
    int size;
    std::cout << "Введите размер массива: ";
    std::cin >> size;
    int* arr = new int[size];

    std::cout << "Введите элементы массива: ";
    for (int i = 0; i < size; i++) {
        std::cin >> arr[i];
    }

    EvenCountAccumulator evenAccumulator;
    OddCountAccumulator oddAccumulator;

    std::cout << "Количество четных чисел: " << evenAccumulator.fold(arr, size) << std::endl;
    std::cout << "Количество нечетных чисел: " << oddAccumulator.fold(arr, size) << std::endl;

    delete[] arr; // Освобождение памяти
    return 0;
}
```

 Задача 5: Подсчет простых и непростых чисел

```cpp
class PrimeCountAccumulator : public IAccumulator {
private:
    bool is_prime(int num) {
        if (num <= 1) return false;
        for (int i = 2; i <= sqrt(num); i++) {
            if (num % i == 0) return false;
        }
        return true;
    }
public:
    int fold(int arr[], int size) override {
        int count = 0;
        for (int i = 0; i < size; i++) {
            if (is_prime(arr[i])) {
                count++;
            }
        }
        return count;
    }
};

class NonPrimeCountAccumulator : public IAccumulator {
public:
    int fold(int arr[], int size) override {
        PrimeCountAccumulator primeCounter;
        int totalCount = size;
        return totalCount - primeCounter.fold(arr, size);
    }
};

int main() {
    int size;
    std::cout << "Введите размер массива: ";
    std::cin >> size;
    int* arr = new int[size];

    std::cout << "Введите элементы массива: ";
    for (int i = 0; i < size; i++) {
        std::cin >> arr[i];
    }

    PrimeCountAccumulator primeAccumulator;
    NonPrimeCountAccumulator nonPrimeAccumulator;

    std::cout << "Количество простых чисел: " << primeAccumulator.fold(arr, size) << std::endl;
    std::cout << "Количество непростых чисел: " << nonPrimeAccumulator.fold(arr, size) << std::endl;

    delete[] arr; // Освобождение памяти
    return 0;
}
```

Задача 6: Анализ строки (строчные и заглавные символы)

```cpp
class IAnalyse {
public:
    virtual int analyse(const std::string& str) = 0; // Чистая виртуальная функция
};

class LowercaseCount : public IAnalyse {
public:
    int analyse(const std::string& str) override {
        int count = 0;
        for (char c : str) {
            if (std::islower(c)) {
                count++;
            }
        }
        return count;
    }
};

class UppercaseCount : public IAnalyse {
public:
    int analyse(const std::string& str) override {
        int count = 0;
        for (char c : str) {
            if (std::isupper(c)) {
                count++;
            }
        }
        return count;
    }
};

int main() {
    std::string input;
    std::cout << "Введите строку (только латинские буквы): ";
    std::cin >> input;

    LowercaseCount lowerCounter;
    UppercaseCount upperCounter;

    std::cout << "Количество строчных символов: " << lowerCounter.analyse(input) << std::endl;
    std::cout << "Количество заглавных символов: " << upperCounter.analyse(input) << std::endl;

    return 0;
}
```

 Задача 7: Анализ строки (гласные и согласные буквы)

```cpp
class VowelCount : public IAnalyse {
public:
    int analyse(const std::string& str) override {
        int count = 0;
        std::string vowels = "aeiouAEIOU";
        for (char c : str) {
            if (vowels.find(c) != std::string::npos) {
                count++;
            }
        }
        return count;
    }
};

class ConsonantCount : public IAnalyse {
public:
    int analyse(const std::string& str) override {
        int count = 0;
        std::string vowels = "aeiouAEIOU";
        for (char c : str) {
            if (std::isalpha(c) && vowels.find(c) == std::string::npos) {
                count++;
            }
        }
        return count;
    }
};

int main() {
    std::cout << "Введите строку (только латинские буквы): ";
    std::cin >> input;

    VowelCount vowelCounter;
    ConsonantCount consonantCounter;

    std::cout << "Количество гласных букв: " << vowelCounter.analyse(input) << std::endl;
    std::cout << "Количество согласных букв: " << consonantCounter.analyse(input) << std::endl;

    return 0;
}
```

 Общие шаги для всех задач

- Каждое задание создаёт интерфейс с одной функцией. Мы реализуем его в соответствующих классах.
- В каждом классе реализуется логика, отвечающая за расчёт или анализ, как указано в заданиях.
- Для демонстрации работы классов мы запрашиваем у пользователя входные данные (числа, массивы или строки) и выводим результаты обработки.

