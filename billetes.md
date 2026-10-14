# Código de billetes.py

```python
def dividir(cantidad, denominaciones, combinacion=[]):
    if cantidad == 0:
        print(" + ".join(map(str, combinacion)))
        return

    for moneda in denominaciones:
        if moneda <= cantidad:
            dividir(cantidad - moneda, denominaciones, combinacion + [moneda])


denominaciones = [100, 50, 20, 10, 5]

cantidad = int(input("Ingresa la cantidad: "))

print(f"\nFormas de dividir {cantidad}:\n")

dividir(cantidad, denominaciones)
```

