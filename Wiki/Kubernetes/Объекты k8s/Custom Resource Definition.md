`CRD` - Объект который позволяет создавать свои собственные объекты kubernetes.
С помощью него мы можем определить новый объект.

```yaml
apiVersion:
kind: CustomResourceDefinition
metadata:
  name: flighttickets.com
spec:
  scope: Namespaced
  group: flights.ru
  names:
    kind: FlightTicket 
	singular: flightticket
	plural: flighttickets
	shortNames:
	 - ft
  versions:
    - name: v1
      served: true
      storage: true
      schema:
        openAPIV3Schema:
	    ...

```

- `scope` - определяет то, к какой абстракции будет относится ресурс. К неймспейсу или ко всему кластеру.
- `names.kind` - блок `kind` нашего объекта - его имя.
- `singular, plural, shortNames` - имена объекта как оно будет использоваться в команде kubectl и выводится в отладочной информации.
- `schema.openAPIV3Schema` - указываются все поля которые могут быть определены в объекте и какого типа их значения.