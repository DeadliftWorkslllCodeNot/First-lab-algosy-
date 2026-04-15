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
