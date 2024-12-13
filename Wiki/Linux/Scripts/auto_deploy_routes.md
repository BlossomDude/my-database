```bash
#!/usr/bin/env bash

hash_commit=$(git log -n 1 --pretty=format:"%H")

echo "Hash commit: $hash_commit"

# Проверяем добавленные роуты
files_list=$(git show --diff-filter=AMD --name-status --pretty="" $hash_commit | grep <pattern>)

# Проверка на наличие роутов.
if [[ -n "$files_list" ]]; then
	echo "Список добавленных и измененных роутов:"
	echo "$files_list"
else
	echo "Измененные, добавленные или удаленные роуты отсутствуют. Завершение скрипта..."
	exit 1
fi

# OpenShift apply/delete
cat ~/workdir/testscript.txt | while IFS= read -r line; do
	action=$(echo "$line" | grep -o [AMD])
	path=$(echo $line | cut -f2 -d " ")
	service_name=$(basename $(echo $path | cut -f1 -d "."))
	namespace=$(echo "$path" | cut -f1 -d "/")
	
	if [[ "$action" == "A" || "$action" == "M" ]]; then
		echo "oc --kubeconfig $OC_TMP -n $namespace apply -f $path"
			if [[ "$bash_test" == "false" ]]; then
				oc --kubeconfig $OC_TMP -n $namespace apply -f $path
			fi 
	elif [[ "$action" == "D" ]]; then
		echo "oc --kubeconfig "$OC_TMP" delete routes -n $namespace -l app=$service_name"
		if [[ "$bash_test" == "false" ]]; then
			oc --kubeconfig="$OC_TMP" delete routes -n $namespace -l app=$service_name
		fi
	fi
done

```
