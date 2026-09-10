# Volúmenes de cuerpos geométricos

```lisp
; 1. Cubo
(defun volumen-cubo (lado)
  (* lado lado lado))

; 2. Prisma rectangular
(defun volumen-prisma-rectangular (largo ancho altura)
  (* largo ancho altura))

; 3. Cilindro
(defun volumen-cilindro (radio altura)
  (* 3.141592653589793 radio radio altura))

; 4. Cono
(defun volumen-cono (radio altura)
  (/ (* 3.141592653589793 radio radio altura) 3))

; 5. Esfera
(defun volumen-esfera (radio)
  (/ (* 4 3.141592653589793 radio radio radio) 3))
```
