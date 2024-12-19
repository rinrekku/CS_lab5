# Лабораторная работа 5

## Основы работы с Git

### Задание:

1. **Автоматизация проверки формата файлов при коммите**.
2. **Использование Git Flow в проекте**.

## Задание 1 - Автоматизация проверки формата файлов при коммите

Для автоматизации проверки формата файлов перед коммитом был создан bash-скрипт с использованием Git Hooks. Этот скрипт проверяет, что добавлены только файлы формата `.txt` , и все они содержат подпись в конце.

### Шаги:
1. Создаём файл `pre-commit` в папке `.git/hooks/`.
2. В файл был добавлен следующий скрипт:

```bash
#!/bin/bash

FILES=$(git diff --cached --name-only)
NONTXTS=$(echo $FILES | grep -v '\.txt$')

if [ "$NONTXTS" ]; then
	echo "Non-.txt format files committed, aborting..."
	exit 1
fi

for FILE in $FILES; do
	if ! tail -n 1 $FILE | grep -q 'Author'; then
		echo "Non-signed .txt files committed, aborting..."
		exit 1
	fi
done

echo "Checks passed, committing..."
exit 0
```

Теперь перед каждым коммитом выполняется проверка файлов `.txt` на наличие подписи автора в конце. Если подпись отсутствует, коммит не выполнится.

---

## Задание 2 - Использование Git Flow в проекте

1. Инициализировал Git Flow в репозитории:

```bash
git flow init
```

2. Создал ветку для новой функциональности `task-management`:

```bash
git flow feature start task-management
```

3. Добавил код для функциональности управления задачами в файл `task_manager.py`:

```python
def create_task(title, description):
    # Логика создания задачи
    print(f"Создана новая задача: {title}")
```

4. Коммит изменений:

```bash
git add task_manager.py
git commit -m "Добавлен функционал управления задачами"
```

5. Завершил разработку функциональности и объединил ветку `task-management` с основной веткой:

```bash
git flow feature finish task-management
```

6. Создал релизную ветку для версии 1.0.0:

```bash
git checkout develop
git flow release start v1.0.0
```

7. Обновил версию в файле `version.txt`:

```bash
echo "v1.0.0" > version.txt
git add version.txt
git commit -m "Updated version"
```

8. Завершил релиз и объединил изменения с ветками `develop` и `main`:

```bash
git flow release finish v1.0.0
```

9. Создал hotfix для исправления критической ошибки:

```bash
git flow hotfix start hotfix-1.0.1
```

10. Создал файл с исправленной критической ошибкой:

```python
def average(numbers):
    #Added check for an empty list
    return sum(numbers) / len(numbers) if numbers else "The list is empty"
```

11. Исправил ошибку и закоммитил изменения:

```bash
git add file_with_error.py
git commit -m "Fixed a critical error"
```

12. Завершил hotfix и объединил изменения с ветками `develop` и `main`:

```bash
git flow hotfix finish hotfix-1.0.1
```

13. Отправил изменения на удаленный репозиторий:

```bash
git push origin develop
git push origin main
```
Таким образом получился данный цикл разработки:

```bash
*   f8311e1 (HEAD -> feature-branch, origin/feature-branch) Merge tag 'hotfix-1.0.1' into feature-branch
|\  
| *   60605fb (tag: hotfix-1.0.1, origin/main, origin/HEAD, main) Merge branch 'hotfix/hotfix-1.0.1' Fixed a critical error
| |\  
| | * 3f60229 Fixed a critical error
| |/  
* | 9236ef2 Merge tag 'v1.0.0' into feature-branch
|\| 
| *   c16c098 (tag: v1.0.0) Merge branch 'release/v1.0.0'
| |\  
| | * 363b3b2 Обновлена версия для релиза v1.0.0
| |/  
|/|   
* | 9068442 Добавле функционал управления задачами
| * d6f3e3e test commit
| * d57effc test commit
| * 1eb4c62 Test commit
| * 2541460 Test commit
| * a96ac98 Test Commit
| * 387dc84 Test
| * 56cf6d3 Test Commit
| * a9250ad Changed the book title
|/  
* b6210ad added a modified version of example.txt
* 908bebb added file example.txt
* 466505b Initial commit

```

---

### Вывод:

Я научился основам работы с `git` и `git flow`, а также познакомился с веточной моделью разработки и особенностями данной системы.
