# First-lab-algosy
# Лабораторная №1

**Выполнила:** Пермякова Екатерина Алексеевна, ИДБ-25-02

---

## 1. Проверка наличия элемента в массиве

**Алгоритмическая сложность:** \( O(n) \)

### Код функции:
```С
bool find_el(int* mass, int size, int num) {
	for (int i = 0; i < size;i++) {
		if (mass[i] == num) return true;
	}
	return false;

}

void fill_rand(int arr[], int size, int min, int max) {
	for (int i = 0; i < size; i++) {
		arr[i] = min + rand() % (max - min + 1);
	}
}

int main() {
	setlocale(LC_ALL, "Russian");
	time_t start, end;

	start = time(NULL);

	int size_m = 10;
	int* mass = (int*)malloc(size_m * sizeof(int));
	if (mass == NULL) {
		printf("Ошибка выделения памяти\n");
		return 1;
	}

	int el_f;
	printf("Введите искомый элемент: ");
	scanf_s("%d", &el_f);
	
	srand(time(NULL));
	fill_rand(mass, size_m, 1, 100);

	bool found = find_el(mass, size_m, el_f);
	if (found) printf("элемент %d найден\n", el_f);
	else printf("элемент не найден\n");

	for (int i = 0; i < 100000000; i++);
	end = time(NULL);

	printf("Время выполнения: %.2f секунд\n", difftime(end, start));
	free(mass);
}
на языке С
# First-lab-algosy
# Лабораторная №1

**Выполнила:** Пермякова Екатерина Алексеевна, ИДБ-25-02

---

## 1. Проверка наличия элемента в массиве

**Алгоритмическая сложность:** \( O(n) \)

### Код функции:
```С
bool find_el(int* mass, int size, int num) {
	for (int i = 0; i < size; i++) {
		if (mass[i] == num) return true;
	}
	return false;

}

void fill_rand(int arr[], int size, int min, int max) {
	for (int i = 0; i < size; i++) {
		arr[i] = min + rand() % (max - min + 1);
	}
}

double measure_time(int* mass, int size, int num, bool* result) {
	clock_t start = clock();
	*result = find_el(mass, size, num);
	clock_t end = clock();
	return (double)(end - start) / CLOCKS_PER_SEC;
}

int main() {
	int sizes[] = { 10, 100, 1000, 10000 };
	int num_s = sizeof(sizes) / sizeof(*sizes);

	srand(time(NULL));

	int el_f;
	printf("Enter the desire element: ");
	scanf_s("%d", &el_f);

	printf("| Size   | Search elem| Status       | Time (sec) |\n");
	printf("|--------|------------|--------------|------------|\n");

	for (int i = 0; i < num_s; i++) {
		int size = sizes[i];
		int* mass = (int*)malloc(size * sizeof(int));
		if (mass == NULL) {
			printf("Memory allocation error for size %d\n", size);
			continue;
		}

		fill_rand(mass, size, 1, 1000);
		bool found;
		double search_time = measure_time(mass, size, el_f, &found);

		printf("| %-6d | %-10d | %-12s | %-10.8f |\n", size, el_f,
			found ? "found  " : "not found  ", search_time);

		free(mass);
	}
}


---
## 2. Поиск второго максимального элемента массива

**Алгоритмическая сложность:** \( O(n) \)

### Код функции:
```С
void fill_rand(int* mass, int size, int min, int max) {
	for (int i = 0; i < size; i++) {
		mass[i] = min + rand() % (max - min + 1);
	}
}

int find_sec_max(int *mass, int size) {
	int max1 = mass[0], max2 = mass[0];
	for (int i = 0; i < size;i++) {
		if (mass[i] > max1) {
			max2=max1;
			max1 = mass[i];
		}
		else if (mass[i]>max2 && mass[i]!=max1) {
			max2 = mass[i];
		}
	}
	return max2;
}

double measure_time(int* mass, int size, int* result) {
	clock_t start = clock();
	*result = find_sec_max(mass, size);
	clock_t end = clock();

	return (double)(end - start) / CLOCKS_PER_SEC;
}

int main() {
	int sizes[] = { 10, 100, 1000, 1000000};
	int num_s = sizeof(sizes) / sizeof(*sizes);

	srand(time(NULL));


	printf("  max2  | %-10s | %-15s\n", "Size", "Total time");
	printf("--------|------------|-----------------\n");

	for (int i = 0; i < num_s; i++) {
		int size = sizes[i];
		int* mass = (int*)malloc(size * sizeof(int));
		if (mass == NULL) {
			printf("Memory allocation error for size %d\n", size);
			continue;
		}

		fill_rand(mass, size, 1, 1000);
		int max2;

		double search_time = measure_time(mass, size, &max2);
		printf("  %-5d | %-10d | %-15.8f\n", max2, size, search_time);

		free(mass);
	}
	return 0;
}

## 3. Бинарный поиск

**Алгоритмическая сложность:** \( O(log(n)) \)

### Код функции:
```С
void fill_sort_rand(int* mass, int size, int min, int max) {
    mass[0] = min + rand() % ((max - min) / 2 + 1);

    for (int i = 1; i < size; i++) {
        int step = 1 + rand() % ((max - mass[i - 1]) / (size - i + 1) + 1);
        mass[i] = step + mass[i - 1];
        if (mass[i] > max) {
            mass[i] = max - rand() % (max / (size - i + 1));
        }
    }
}

bool binary_search(int* mass, int l, int r, int el) {
    while (l <= r) {
        int mid = l + (r - l) / 2;

        if (mass[mid] == el)
            return true;
        else if (mass[mid] < el)
            l = mid + 1;
        else
            r = mid - 1;
    }
    return false;
}

double measure_time(int* mass, int size, int el, bool* result) {
    clock_t start = clock();
    *result = binary_search(mass, 0, size - 1, el);
    clock_t end = clock();

    return (double)(end - start) / CLOCKS_PER_SEC;
}

int main() {
    int sizes[] = { 10, 100, 1000, 10000};
    int num_s = sizeof(sizes) / sizeof(*sizes);

    srand(time(NULL));

    int el_f;
    printf("Enter the desired element: ");
    scanf_s("%d", &el_f);

    printf("\n");
    printf("| %-10s | %-10s | %-12s | %-15s |\n", "Size", "Element", "Status", "Time (sec)");
    printf("|------------|------------|--------------|-----------------|\n");

    for (int i = 0; i < num_s; i++) {
        int size = sizes[i];
        int* mass = (int*)malloc(size * sizeof(int));

        if (mass == NULL) {
            printf("Memory allocation error for size %d\n", size);
            continue;
        }

        fill_sort_rand(mass, size, 1, 100000);

        bool found;
        double search_time = measure_time(mass, size, el_f, &found);

        printf("| %-10d | %-10d | %-12s | %-15.8f |\n", size, el_f, found ? "found" : "not found", search_time);

        free(mass);
    }

    printf("\n");
    return 0;
}

## 4. Построение таблицы умножения

**Алгоритмическая сложность:** \( O(n^2) \)

### Код функции:
```С
void table(int n) {
    int t;
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= n; j++) {
            t= i * j;
        }
    }
}

double measure_time(int n) {
    clock_t start = clock();
    table(n);
    clock_t end = clock();

    return (double)(end - start) / CLOCKS_PER_SEC;
}

int main() {
    int sizes[] = {5, 10, 100 };
    int num_s = sizeof(sizes) / sizeof(*sizes);

    printf("\n");
    printf("| %-10s | %-15s |\n", "Table Size", "Time (sec)");
    printf("|------------|-----------------|\n");

    for (int i = 0; i < num_s; i++) {
        int n = sizes[i];

        printf("| %-10d | ", n);

        double table_time = measure_time(n);

        printf("%-15.12f |\n", table_time);
    }

    printf("\n");
    return 0;
}

## 5. 

**Алгоритмическая сложность:** \( O(n*k) \)

### Код функции:
```С
int find_max(int* mass, int size) {
    int max = mass[0];
    for (int i = 1; i < size; i++) {
        if (mass[i] > max) {
            max = mass[i];
        }
    }
    return max;
}

void counting_sort_by_digit(int* mass, int size, int exp) {
    int* output = (int*)malloc(size * sizeof(int));
    int count[10] = { 0 };

    for (int i = 0; i < size; i++) {
        int digit = (mass[i] / exp) % 10;
        count[digit]++;
    }

    for (int i = 1; i < 10; i++) {
        count[i] += count[i - 1];
    }

    for (int i = size - 1; i >= 0; i--) {
        int digit = (mass[i] / exp) % 10;
        output[count[digit] - 1] = mass[i];
        count[digit]--;
    }

    for (int i = 0; i < size; i++) {
        mass[i] = output[i];
    }

    free(output);
}

void radix_sort(int* mass, int size) {
    if (size <= 1) return;

    int max_val = find_max(mass, size);

    for (int exp = 1; max_val / exp > 0; exp *= 10) {
        counting_sort_by_digit(mass, size, exp);
    }
}

void fill_rand(int* mass, int size, int min, int max) {
    for (int i = 0; i < size; i++) {
        mass[i] = min + rand() % (max - min + 1);
    }
}

void measure_radix_sort(int* mass, int size) {
    
    clock_t start = clock();
    radix_sort(mass, size);
    clock_t end = clock();

    double time_sec = (double)(end - start) / CLOCKS_PER_SEC;

    // Память = output массив (size * sizeof(int)) + count массив (40 байт)
    size_t memory_bytes = size * sizeof(int) + 40;

    printf("| %-10d | %-15.8f | %-15zu |\n", size, time_sec, memory_bytes);
}

int main() {
    int sizes[] = { 10, 100, 1000, 10000, 100000 };
    int num_s = sizeof(sizes) / sizeof(sizes[0]);

    srand(time(NULL));

    printf("\n");
    printf("| %-10s | %-15s | %-15s |\n",
        "Size", "Time (sec)", "Memory (bytes)");
    printf("|------------|-----------------|-----------------|\n");

    for (int i = 0; i < num_s; i++) {
        int size = sizes[i];
        int* mass = (int*)malloc(size * sizeof(int));

        if (mass == NULL) {
            printf("Memory allocation error for size %d\n", size);
            continue;
        }

        for (int j = 0; j < size; j++) {
            mass[j] = rand() % 100000;
        }

        measure_radix_sort(mass, size);
        free(mass);
    }

    return 0;
}
