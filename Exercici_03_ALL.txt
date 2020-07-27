#Crear DDBB
DROP DATABASE IF EXISTS tienda;
CREATE DATABASE tienda CHARACTER SET utf8mb4;
USE tienda;

CREATE TABLE fabricante (
  codigo INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(100) NOT NULL
);

CREATE TABLE producto (
  codigo INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(100) NOT NULL,
  precio DOUBLE NOT NULL,
  codigo_fabricante INT UNSIGNED NOT NULL,
  FOREIGN KEY (codigo_fabricante) REFERENCES fabricante(codigo)
);

INSERT INTO fabricante VALUES(1, 'Asus');
INSERT INTO fabricante VALUES(2, 'Lenovo');
INSERT INTO fabricante VALUES(3, 'Hewlett-Packard');
INSERT INTO fabricante VALUES(4, 'Samsung');
INSERT INTO fabricante VALUES(5, 'Seagate');
INSERT INTO fabricante VALUES(6, 'Crucial');
INSERT INTO fabricante VALUES(7, 'Gigabyte');
INSERT INTO fabricante VALUES(8, 'Huawei');
INSERT INTO fabricante VALUES(9, 'Xiaomi');

INSERT INTO producto VALUES(1, 'Disco duro SATA3 1TB', 86.99, 5);
INSERT INTO producto VALUES(2, 'Memoria RAM DDR4 8GB', 120, 6);
INSERT INTO producto VALUES(3, 'Disco SSD 1 TB', 150.99, 4);
INSERT INTO producto VALUES(4, 'GeForce GTX 1050Ti', 185, 7);
INSERT INTO producto VALUES(5, 'GeForce GTX 1080 Xtreme', 755, 6);
INSERT INTO producto VALUES(6, 'Monitor 24 LED Full HD', 202, 1);
INSERT INTO producto VALUES(7, 'Monitor 27 LED Full HD', 245.99, 1);
INSERT INTO producto VALUES(8, 'Portátil Yoga 520', 559, 2);
INSERT INTO producto VALUES(9, 'Portátil Ideapd 320', 444, 2);
INSERT INTO producto VALUES(10, 'Impresora HP Deskjet 3720', 59.99, 3);
INSERT INTO producto VALUES(11, 'Impresora HP Laserjet Pro M26nw', 180, 3);

# 1.1.3 Consultas sobre una tabla
USE tienda; #Activamos la DDBB

#Lista el nombre de todos los productos que hay en la tabla producto.
SELECT DISTINCT nombre
FROM producto;

#Lista los nombres y los precios de todos los productos de la tabla producto.
SELECT DISTINCT nombre, precio
FROM producto;

#Lista todas las columnas de la tabla producto.
SELECT *
FROM producto;
DESCRIBE producto; # lista nombre de columnas y propiedades

#Lista el nombre de los productos, el precio en euros y el precio en dólares estadounidenses (USD).
SELECT nombre, precio, round(precio/0.88,2)
FROM producto;

#Lista el nombre de los productos, el precio en euros y el precio en dólares estadounidenses (USD). Utiliza los siguientes alias para las columnas: nombre de producto, euros, dólares.
SELECT nombre as "nombre de producto", precio as euros, round(precio/0.88,2) as dolares
FROM producto;

#Lista los nombres y los precios de todos los productos de la tabla producto, convirtiendo los nombres a mayúscula.
SELECT UPPER(nombre), precio
FROM producto; 

#Lista los nombres y los precios de todos los productos de la tabla producto, convirtiendo los nombres a minúscula.
SELECT LOWER(nombre), precio
FROM producto; 

#Lista el nombre de todos los fabricantes en una columna, y en otra columna obtenga en mayúsculas los dos primeros caracteres del nombre del fabricante.
SELECT nombre,UPPER(LEFT(nombre,2))
FROM fabricante; 

#Lista los nombres y los precios de todos los productos de la tabla producto, redondeando el valor del precio.
SELECT nombre, ROUND(precio)
FROM producto;

#Lista los nombres y los precios de todos los productos de la tabla producto, truncando el valor del precio para mostrarlo sin ninguna cifra decimal.
SELECT nombre, TRUNCATE(precio,0)
FROM producto;

#Lista el código de los fabricantes que tienen productos en la tabla producto.
SELECT codigo_fabricante 
FROM producto;

#Lista el código de los fabricantes que tienen productos en la tabla producto, eliminando los códigos que aparecen repetidos.
SELECT DISTINCT codigo_fabricante 
FROM producto;

#Lista los nombres de los fabricantes ordenados de forma ascendente.
SELECT *
FROM fabricante
ORDER BY nombre ASC;

#Lista los nombres de los fabricantes ordenados de forma descendente.
SELECT *
FROM fabricante
ORDER BY nombre DESC;

#Lista los nombres de los productos ordenados en primer lugar por el nombre de forma ascendente y en segundo lugar por el precio de forma descendente.
SELECT *
FROM producto
ORDER BY nombre ASC, precio DESC;

#Devuelve una lista con las 5 primeras filas de la tabla fabricante.
SELECT *
FROM fabricante
LIMIT 5;

#Devuelve una lista con 2 filas a partir de la cuarta fila de la tabla fabricante. La cuarta fila también se debe incluir en la respuesta.
SELECT *
FROM fabricante
LIMIT 2 OFFSET 3;

#Lista el nombre y el precio del producto más barato. (Utilice solamente las cláusulas ORDER BY y LIMIT)
SELECT nombre, precio
FROM producto
ORDER BY precio ASC;

#Lista el nombre y el precio del producto más caro. (Utilice solamente las cláusulas ORDER BY y LIMIT)
SELECT nombre, precio
FROM producto
ORDER BY precio DESC;

#Lista el nombre de todos los productos del fabricante cuyo código de fabricante es igual a 2.
SELECT nombre, precio
FROM producto
WHERE codigo_fabricante=2
ORDER BY precio ASC;

#Lista el nombre de los productos que tienen un precio menor o igual a 120€.
SELECT nombre, precio
FROM producto
WHERE precio<=120
ORDER BY precio ASC;

#Lista el nombre de los productos que tienen un precio mayor o igual a 400€.
SELECT nombre, precio
FROM producto
WHERE precio>=400
ORDER BY precio ASC;

#Lista el nombre de los productos que no tienen un precio mayor o igual a 400€.
SELECT nombre, precio
FROM producto
WHERE precio<400
ORDER BY precio ASC;

#Lista todos los productos que tengan un precio entre 80€ y 300€. Sin utilizar el operador BETWEEN.
SELECT nombre, precio
FROM producto
WHERE precio>=80 AND precio<=300
ORDER BY precio ASC;

#Lista todos los productos que tengan un precio entre 60€ y 200€. Utilizando el operador BETWEEN.
SELECT nombre, precio
FROM producto
WHERE precio BETWEEN 60 AND 200
ORDER BY precio ASC;

#Lista todos los productos que tengan un precio mayor que 200€ y que el código de fabricante sea igual a 6.
SELECT *
FROM producto
WHERE precio >200 AND codigo_fabricante=6
ORDER BY precio ASC;

#Lista todos los productos donde el código de fabricante sea 1, 3 o 5. Sin utilizar el operador IN.
SELECT *
FROM producto
WHERE codigo_fabricante=1 OR codigo_fabricante=3 OR codigo_fabricante=5;

#Lista todos los productos donde el código de fabricante sea 1, 3 o 5. Utilizando el operador IN.
SELECT *
FROM producto
WHERE codigo_fabricante in (1,3,5);

#Lista el nombre y el precio de los productos en céntimos (Habrá que multiplicar por 100 el valor del precio). Cree un alias para la columna que contiene el precio que se llame céntimos.
SELECT nombre, precio, precio*100 as centimos
FROM producto;

#Lista los nombres de los fabricantes cuyo nombre empiece por la letra S.
SELECT *
FROM fabricante
WHERE nombre LIKE "S%";

#Lista los nombres de los fabricantes cuyo nombre termine por la vocal e.
SELECT *
FROM fabricante
WHERE nombre LIKE "%e";

#Lista los nombres de los fabricantes cuyo nombre contenga el carácter w.
SELECT *
FROM fabricante
WHERE nombre LIKE "%w%";

#Lista los nombres de los fabricantes cuyo nombre sea de 4 caracteres.
SELECT *
FROM fabricante
WHERE nombre LIKE "____";

#Devuelve una lista con el nombre de todos los productos que contienen la cadena Portátil en el nombre.
SELECT *
FROM producto
WHERE nombre LIKE "%portatil%";

#Devuelve una lista con el nombre de todos los productos que contienen la cadena Monitor en el nombre y tienen un precio inferior a 215 €.
SELECT *
FROM producto
WHERE nombre LIKE "%monitor%" AND
precio <215;

#Lista el nombre y el precio de todos los productos que tengan un precio mayor o igual a 180€. Ordene el resultado en primer lugar por el precio (en orden descendente) y en segundo lugar por el nombre (en orden ascendente).
SELECT *
FROM producto
WHERE precio>180
ORDER BY precio DESC, nombre ASC;

#1.1.4 Consultas multitabla (Composición interna)
#Resuelva todas las consultas utilizando la sintaxis de SQL1 y SQL2.

#Devuelve una lista con el nombre del producto, precio y nombre de fabricante de todos los productos de la base de datos.
SELECT p.nombre, p.precio, f.nombre as fabricante
FROM producto as p
JOIN fabricante as f
	ON p.codigo_fabricante=f.codigo;

#Devuelve una lista con el nombre del producto, precio y nombre de fabricante de todos los productos de la base de datos. Ordene el resultado por el nombre del fabricante, por orden alfabético.
SELECT p.nombre, p.precio, f.nombre as fabricante
FROM producto as p
JOIN fabricante as f
	ON p.codigo_fabricante=f.codigo
ORDER BY fabricante ASC;

#Devuelve una lista con el código del producto, nombre del producto, código del fabricante y nombre del fabricante, de todos los productos de la base de datos.
SELECT p.codigo as cod_prod, p.nombre, p.precio, f.codigo as cod_fabricante, f.nombre as fabricante
FROM producto as p
JOIN fabricante as f
	ON p.codigo_fabricante=f.codigo;

#Devuelve el nombre del producto, su precio y el nombre de su fabricante, del producto más barato.
SELECT p.nombre, MIN(p.precio), f.nombre as fabricante
FROM producto as p
JOIN fabricante as f
	ON p.codigo_fabricante=f.codigo;

#Devuelve el nombre del producto, su precio y el nombre de su fabricante, del producto más caro.
SELECT p.nombre, MAX(p.precio), f.nombre as fabricante
FROM producto as p
JOIN fabricante as f
	ON p.codigo_fabricante=f.codigo;

#Devuelve una lista de todos los productos del fabricante Lenovo.
SELECT p.nombre, p.precio, f.nombre as fabricante
FROM producto as p
JOIN fabricante as f
	ON p.codigo_fabricante=f.codigo
WHERE f.nombre LIKE "lenovo";

#Devuelve una lista de todos los productos del fabricante Crucial que tengan un precio mayor que 200€.
SELECT p.nombre, p.precio, f.nombre as fabricante
FROM producto as p
JOIN fabricante as f
	ON p.codigo_fabricante=f.codigo
WHERE f.nombre LIKE "crucial"
HAVING p.precio>200;

#Devuelve un listado con todos los productos de los fabricantes Asus, Hewlett-Packard y Seagate. Sin utilizar el operador IN.
SELECT p.nombre, f.nombre as fabricante
FROM producto as p
JOIN fabricante as f
	ON p.codigo_fabricante=f.codigo
WHERE f.nombre LIKE "asus" OR
f.nombre LIKE "hewlett-packard" OR
f.nombre LIKE "seagate";

#Devuelve un listado con todos los productos de los fabricantes Asus, Hewlett-Packardy Seagate. Utilizando el operador IN.
SELECT p.nombre, f.nombre as fabricante
FROM producto as p
JOIN fabricante as f
	ON p.codigo_fabricante=f.codigo
WHERE f.nombre IN ("asus","hewlett-packard","seagate");

#Devuelve un listado con el nombre y el precio de todos los productos de los fabricantes cuyo nombre termine por la vocal e.
SELECT p.nombre, p.precio, f.nombre as fabricante
FROM producto as p
JOIN fabricante as f
	ON p.codigo_fabricante=f.codigo
WHERE f.nombre LIKE "%e";

#Devuelve un listado con el nombre y el precio de todos los productos cuyo nombre de fabricante contenga el carácter w en su nombre.
SELECT p.nombre, p.precio, f.nombre as fabricante
FROM producto as p
JOIN fabricante as f
	ON p.codigo_fabricante=f.codigo
WHERE f.nombre LIKE "%w%";

#Devuelve un listado con el nombre de producto, precio y nombre de fabricante, de todos los productos que tengan un precio mayor o igual a 180€. 
#Ordene el resultado en primer lugar por el precio (en orden descendente) y en segundo lugar por el nombre (en orden ascendente)
SELECT p.nombre, p.precio, f.nombre as fabricante
FROM producto as p
JOIN fabricante as f
	ON p.codigo_fabricante=f.codigo
WHERE p.precio >=180
ORDER BY p.precio DESC, p.nombre DESC;

#Devuelve un listado con el código y el nombre de fabricante, solamente de aquellos fabricantes que tienen productos asociados en la base de datos.
SELECT DISTINCT f.codigo, f.nombre
FROM fabricante as f
JOIN producto as p
ON p.codigo_fabricante=f.codigo;

#1.1.5 Consultas multitabla (Composición externa)
#Resuelva todas las consultas utilizando las cláusulas LEFT JOIN y RIGHT JOIN.

#Devuelve un listado de todos los fabricantes que existen en la base de datos, junto con los productos que tiene cada uno de ellos. El listado deberá mostrar también aquellos fabricantes que no tienen productos asociados.
SELECT *
FROM fabricante as f 
LEFT JOIN producto as p
ON p.codigo_fabricante=f.codigo;

#Devuelve un listado donde sólo aparezcan aquellos fabricantes que no tienen ningún producto asociado.
SELECT *
FROM fabricante as f 
LEFT JOIN producto as p
ON p.codigo_fabricante=f.codigo
WHERE p.codigo IS NULL;

#¿Pueden existir productos que no estén relacionados con un fabricante? Justifique su respuesta.
"Cada producto exite por que alguien lo fabrico! Lo que puede ocurrir es que no sea de una marca 'conocida' y no se etiquete/indique. 
El termino 'OEM: Original Equipment Manufacturer' se utuliza para designar productos genericos, originales, pero sin marca. 
Luego estos productos son comprados por marcas que revenden los productos a precios de minorista. 
Un fabricante OEM en china fabrica cables y luego son comprados por marcas como Apple o Samsung para ser revendidos"

#1.1.6 Consultas resumen
#Calcula el número total de productos que hay en la tabla productos.
SELECT COUNT(nombre)
FROM producto;

#Calcula el número total de fabricantes que hay en la tabla fabricante.
SELECT COUNT(nombre)
FROM fabricante;

#Calcula el número de valores distintos de código de fabricante aparecen en la tabla productos.
SELECT COUNT(DISTINCT codigo_fabricante)
FROM producto;

#Calcula la media del precio de todos los productos.
SELECT AVG(precio)
FROM producto;

#Calcula el precio más barato de todos los productos.
SELECT MIN(precio)
FROM producto;

#Calcula el precio más caro de todos los productos.
SELECT MAX(precio)
FROM producto;

#Lista el nombre y el precio del producto más barato.
SELECT nombre, precio
FROM producto
WHERE precio = (SELECT MIN(precio) from producto);

#o

SELECT nombre, precio
FROM producto
ORDER BY precio ASC
LIMIT 1;

#Lista el nombre y el precio del producto más caro.
SELECT nombre, precio
FROM producto
WHERE precio = (SELECT MAX(precio) from producto);

#Calcula la suma de los precios de todos los productos.
SELECT SUM(precio)
FROM producto;

#Calcula el número de productos que tiene el fabricante Asus.
SELECT count(p.nombre)
FROM producto as p
JOIN fabricante as f
ON p.codigo_fabricante=f.codigo
WHERE f.nombre LIKE "Asus";

#Calcula la media del precio de todos los productos del fabricante Asus.
SELECT AVG(p.precio)
FROM producto as p
JOIN fabricante as f
ON p.codigo_fabricante=f.codigo
WHERE f.nombre LIKE "Asus";

#Calcula el precio más barato de todos los productos del fabricante Asus.
SELECT MIN(p.precio)
FROM producto as p
JOIN fabricante as f
ON p.codigo_fabricante=f.codigo
WHERE f.nombre LIKE "Asus";

#Calcula el precio más caro de todos los productos del fabricante Asus.
SELECT MAX(p.precio)
FROM producto as p
JOIN fabricante as f
ON p.codigo_fabricante=f.codigo
WHERE f.nombre LIKE "Asus";

#Calcula la suma de todos los productos del fabricante Asus.
SELECT SUM(p.precio)
FROM producto as p
JOIN fabricante as f
ON p.codigo_fabricante=f.codigo
WHERE f.nombre LIKE "Asus";

#Muestra el precio máximo, precio mínimo, precio medio y el número total de productos que tiene el fabricante Crucial.
SELECT MAX(p.precio), MIN(p.precio), AVG(p.precio), COUNT(p.nombre)
FROM producto as p
JOIN fabricante as f
ON p.codigo_fabricante=f.codigo
WHERE f.nombre LIKE "Crucial";

#Muestra el número total de productos que tiene cada uno de los fabricantes. El listado también debe incluir los fabricantes que no tienen ningún producto. 
#El resultado mostrará dos columnas, una con el nombre del fabricante y otra con el número de productos que tiene. Ordene el resultado descendentemente por el número de productos.
SELECT f.nombre, count(p.nombre) as n_prod
FROM fabricante as f
LEFT JOIN producto as p
ON p.codigo_fabricante=f.codigo
GROUP BY f.nombre
ORDER BY n_prod DESC;

#Muestra el precio máximo, precio mínimo y precio medio de los productos de cada uno de los fabricantes. El resultado mostrará el nombre del fabricante junto con los datos que se solicitan.
SELECT MAX(p.precio), MIN(p.precio), AVG(p.precio), f.nombre
FROM producto as p
JOIN fabricante as f
ON p.codigo_fabricante=f.codigo
GROUP BY f.nombre
ORDER BY f.nombre ASC;

#Muestra el precio máximo, precio mínimo, precio medio y el número total de productos de los fabricantes que tienen un precio medio superior a 200€. No es necesario mostrar el nombre del fabricante, con el código del fabricante es suficiente.
SELECT MAX(p.precio), MIN(p.precio), AVG(p.precio) as precio_medio, COUNT(p.nombre) as n_prod, codigo_fabricante
FROM producto as p
GROUP BY codigo_fabricante
HAVING precio_medio>200
ORDER BY precio_medio ASC;

#Muestra el nombre de cada fabricante, junto con el precio máximo, precio mínimo, precio medio y el número total de productos de los fabricantes que tienen un precio medio superior a 200€. Es necesario mostrar el nombre del fabricante.
SELECT f.nombre as fabricante, MAX(p.precio), MIN(p.precio), AVG(p.precio) as precio_medio, COUNT(p.nombre) as n_prod
FROM producto as p
JOIN fabricante as f
ON p.codigo_fabricante=f.codigo
GROUP BY fabricante
HAVING precio_medio>200
ORDER BY precio_medio ASC;

#Calcula el número de productos que tienen un precio mayor o igual a 180€.
SELECT count(nombre)
FROM producto
WHERE precio>=180;

#Calcula el número de productos que tiene cada fabricante con un precio mayor o igual a 180€.
SELECT codigo_fabricante,count(nombre)
FROM producto
WHERE precio>=180
GROUP BY codigo_fabricante;

#Lista el precio medio los productos de cada fabricante, mostrando solamente el código del fabricante.
SELECT codigo_fabricante, AVG(precio)
FROM producto
GROUP BY codigo_fabricante;

#Lista el precio medio los productos de cada fabricante, mostrando solamente el nombre del fabricante.
SELECT f.nombre, AVG(p.precio)
FROM producto as p
JOIN fabricante as f
ON p.codigo_fabricante=f.codigo
GROUP BY f.nombre;

#Lista los nombres de los fabricantes cuyos productos tienen un precio medio mayor o igual a 150€.
SELECT f.nombre, AVG(p.precio)
FROM producto as p
JOIN fabricante as f
ON p.codigo_fabricante=f.codigo
GROUP BY f.nombre
HAVING AVG(p.precio)>=150;

#Devuelve un listado con los nombres de los fabricantes que tienen 2 o más productos.
SELECT f.nombre as fabricante, COUNT(p.nombre) as n_prod
FROM producto as p
JOIN fabricante as f
ON p.codigo_fabricante=f.codigo
GROUP BY fabricante
HAVING n_prod>=2;

#Devuelve un listado con los nombres de los fabricantes y el número de productos que tiene cada uno con un precio superior o igual a 220 €. 
#No es necesario mostrar el nombre de los fabricantes que no tienen productos que cumplan la condición.
SELECT f.nombre as fabricante, COUNT(p.nombre) as n_prod
FROM producto as p
JOIN fabricante as f
ON p.codigo_fabricante=f.codigo
WHERE p.precio>=220
GROUP BY fabricante
ORDER BY n_prod DESC;

#Devuelve un listado con los nombres de los fabricantes y el número de productos que tiene cada uno con un precio superior o igual a 220 €. 
#El listado debe mostrar el nombre de todos los fabricantes, es decir, si hay algún fabricante que no tiene productos con un precio superior o igual a 220€ deberá aparecer en el listado con un valor igual a 0 en el número de productos.
SELECT f.nombre, COUNT(p.nombre)
FROM fabricante as f
LEFT JOIN (SELECT * FROM producto WHERE precio>=220) as p
ON p.codigo_fabricante=f.codigo
GROUP BY f.nombre;

#Devuelve un listado con los nombres de los fabricantes donde la suma del precio de todos sus productos es superior a 1000 €.
SELECT f.nombre
FROM producto as p
JOIN fabricante as f
ON p.codigo_fabricante=f.codigo
GROUP BY f.nombre
HAVING SUM(p.precio)>1000 ;

#Devuelve un listado con el nombre del producto más caro que tiene cada fabricante. El resultado debe tener tres columnas: nombre del producto, precio y nombre del fabricante. 
#El resultado tiene que estar ordenado alfabéticamente de menor a mayor por el nombre del fabricante.
SELECT p.nombre, MAX(p.precio), f.nombre
FROM producto as p
JOIN fabricante as f
ON p.codigo_fabricante=f.codigo
GROUP BY f.nombre
ORDER BY f.nombre ASC;

#1.1.7 Subconsultas (En la cláusula WHERE)
#1.1.7.1 Con operadores básicos de comparación
#Devuelve todos los productos del fabricante Lenovo. (Sin utilizar INNER JOIN).
SELECT nombre
FROM producto
WHERE codigo_fabricante=(SELECT codigo FROM fabricante WHERE nombre='Lenovo');

#Devuelve todos los datos de los productos que tienen el mismo precio que el producto más caro del fabricante Lenovo. (Sin utilizar INNER JOIN).
SELECT *
FROM producto
WHERE precio=(SELECT max(precio) FROM producto WHERE codigo_fabricante=(SELECT codigo FROM fabricante WHERE nombre='Lenovo'));

#Lista el nombre del producto más caro del fabricante Lenovo.
SELECT nombre
FROM producto
wHERE precio=(SELECT precio FROM producto WHERE codigo_fabricante=(SELECT codigo FROM fabricante WHERE nombre='Lenovo') HAVING precio=MAX(precio))
AND codigo_fabricante=(SELECT codigo FROM fabricante WHERE nombre='Lenovo');

#Lista el nombre del producto más barato del fabricante Hewlett-Packard.
SELECT nombre
FROM producto
WHERE precio=(SELECT precio FROM producto WHERE codigo_fabricante=(SELECT codigo FROM fabricante WHERE nombre='Hewlett-Packard') HAVING precio=MIN(precio))
AND codigo_fabricante=(SELECT codigo FROM fabricante WHERE nombre='Hewlett-Packard');

#Devuelve todos los productos de la base de datos que tienen un precio mayor o igual al producto más caro del fabricante Lenovo.
SELECT nombre
FROM producto
wHERE precio>=(SELECT precio FROM producto WHERE codigo_fabricante=(SELECT codigo FROM fabricante WHERE nombre='Lenovo') HAVING precio=MAX(precio));

#Lista todos los productos del fabricante Asus que tienen un precio superior al precio medio de todos sus productos.
SELECT nombre
FROM producto
WHERE precio>(SELECT AVG(precio) FROM producto WHERE codigo_fabricante=(SELECT codigo FROM fabricante WHERE nombre='Asus'))
AND codigo_fabricante=(SELECT codigo FROM fabricante WHERE nombre='Asus');

#1.1.7.2 Subconsultas con ALL y ANY
#Devuelve el producto más caro que existe en la tabla producto sin hacer uso de MAX, ORDER BY ni LIMIT.
SELECT nombre FROM producto
WHERE precio >= ALL(SELECT precio FROM producto);

#Devuelve el producto más barato que existe en la tabla producto sin hacer uso de MIN, ORDER BY ni LIMIT.
SELECT nombre FROM producto
WHERE precio <= ALL(SELECT precio FROM producto);

#Devuelve los nombres de los fabricantes que tienen productos asociados. (Utilizando ALL o ANY).
SELECT * FROM fabricante WHERE codigo = ANY (SELECT codigo_fabricante FROM producto);

#Devuelve los nombres de los fabricantes que no tienen productos asociados. (Utilizando ALL o ANY).
SELECT * FROM fabricante WHERE codigo != ALL (SELECT codigo_fabricante FROM producto);

#1.1.7.3 Subconsultas con IN y NOT IN
#Devuelve los nombres de los fabricantes que tienen productos asociados. (Utilizando IN o NOT IN).
SELECT * FROM fabricante WHERE codigo IN (SELECT codigo_fabricante FROM producto);

#Devuelve los nombres de los fabricantes que no tienen productos asociados. (Utilizando IN o NOT IN).
SELECT * FROM fabricante WHERE codigo NOT IN (SELECT codigo_fabricante FROM producto);

#1.1.7.4 Subconsultas con EXISTS y NOT EXISTS
#Devuelve los nombres de los fabricantes que tienen productos asociados. (Utilizando EXISTS o NOT EXISTS).
SELECT nombre FROM fabricante WHERE EXISTS 
(SELECT codigo_fabricante FROM producto WHERE producto.codigo_fabricante = fabricante.codigo);

#Devuelve los nombres de los fabricantes que no tienen productos asociados. (Utilizando EXISTS o NOT EXISTS).
SELECT nombre FROM fabricante WHERE NOT EXISTS 
(SELECT codigo_fabricante FROM producto WHERE producto.codigo_fabricante = fabricante.codigo);

#1.1.7.5 Subconsultas correlacionadas
#Lista el nombre de cada fabricante con el nombre y el precio de su producto más caro.
SELECT f.nombre, p.nombre, p.precio
FROM fabricante f, producto p
WHERE p.codigo_fabricante = f.codigo 
AND p.precio =
	(SELECT MAX(p2.precio) 
	FROM producto p2 
	WHERE p2.codigo_fabricante = f.codigo)
    ORDER BY f.nombre ASC;

#Devuelve un listado de todos los productos que tienen un precio mayor o igual a la media de todos los productos de su mismo fabricante.
SELECT f.nombre, p.nombre, p.precio
FROM fabricante f, producto p
WHERE p.codigo_fabricante = f.codigo 
AND p.precio >=
	(SELECT AVG(p2.precio) 
	FROM producto p2 
	WHERE p2.codigo_fabricante = f.codigo)
    ORDER BY f.nombre ASC;

#Lista el nombre del producto más caro del fabricante Lenovo.
SELECT p.nombre, p.precio, f.nombre
FROM fabricante f, producto p
WHERE p.codigo_fabricante = f.codigo 
AND p.precio =
	(SELECT MAX(p2.precio) 
	FROM producto p2 
	WHERE p2.codigo_fabricante = f.codigo AND f.nombre ='Lenovo');

#1.1.8 Subconsultas (En la cláusula HAVING)
#Devuelve un listado con todos los nombres de los fabricantes que tienen el mismo número de productos que el fabricante Lenovo.
#Como dijo jack el destripador; vamos por partes :)
SELECT codigo FROM fabricante WHERE nombre='Lenovo'; #Devuelve el codigo de Levono
SELECT codigo_fabricante, count(codigo_fabricante) FROM producto GROUP BY codigo_fabricante; #Devuelve tabla con numero de articulos por fabricante
SELECT count(codigo_fabricante) FROM producto WHERE codigo_fabricante=(SELECT codigo FROM fabricante WHERE nombre='Lenovo') GROUP BY codigo_fabricante; #Numero de articulos de Levono

#Finalmente, todo junto:
SELECT f.nombre, count(p.codigo_fabricante) n_prod
FROM producto p 
INNER JOIN fabricante f
ON p.codigo_fabricante=f.codigo
GROUP BY p.codigo_fabricante
HAVING n_prod=(SELECT count(codigo_fabricante) FROM producto WHERE codigo_fabricante=(SELECT codigo FROM fabricante WHERE nombre='Lenovo') GROUP BY codigo_fabricante);