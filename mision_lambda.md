# Misión 1 — El Expediente Desordenado

```lisp

;;; DATOS DE LOS AGENTES


(setq *agentes*
      '((ana    28 3 morelia   120)
        (beto   35 5 uruapan   340)
        (carla  22 1 morelia    45)
        (diego  41 4 zamora    210)
        (elena  30 2 patzcuaro  90)
        (fausto 26 5 morelia   400)))


;;; 1. COMPROBAR LA TABLA DE PREDICCIONES


(format t "~%Tabla de predicciones~%")

(format t "a: ~S~%"
        (car (cdr (car *agentes*))))

(format t "b: ~S~%"
        (car (car (cdr *agentes*))))

(format t "c: ~S~%"
        (cdr (car (cdr (cdr *agentes*)))))

(format t "d: ~S~%"
        (car (cdr (cdr (cdr
          (car (cdr (cdr (cdr *agentes*)))))))))

(format t "e: ~S~%"
        (car (cdr (cdr (car (cdr *agentes*))))))

(format t "f: ~S~%"
        (car (cdr (cdr
          (car (cdr (cdr (cdr (cdr *agentes*)))))))))



;;; 2. FUNCIONES DE ACCESO


(defun nombre (agente)
  (car agente))

(defun edad (agente)
  (car (cdr agente)))

(defun nivel (agente)
  (car (cdr (cdr agente))))

(defun base (agente)
  (car (cdr (cdr (cdr agente)))))

(defun puntos (agente)
  (car (cdr (cdr (cdr (cdr agente))))))

;;;primer agente: Ana

(format t "~%Funciones de acceso: Ana~%")
(format t "Nombre: ~S~%" (nombre (car *agentes*)))
(format t "Edad: ~S~%"   (edad (car *agentes*)))
(format t "Nivel: ~S~%"  (nivel (car *agentes*)))
(format t "Base: ~S~%"   (base (car *agentes*)))
(format t "Puntos: ~S~%" (puntos (car *agentes*)))


;;; 3. OBTENER LOS PUNTOS DE ELENA


(format t "~%Puntos de Elena~%")
(format t "~S~%"
        (puntos
          (car (cdr (cdr (cdr (cdr *agentes*)))))))


;;; 4. REGISTRO INCOMPLETO


(format t "~%Registro incompleto~%")
(format t "~S~%" (car (cdr '(ana))))

;;; Devuelve NIL y no produce error
;;; (cdr '(ana)) devuelve nil
;;; (car nil) tambien devuelve NIL.

```
