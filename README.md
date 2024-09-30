-- Hány eladott termékből hányat nem hoztak be cserére?

SELECT COUNT(*)
FROM ELADÁS e
LEFT JOIN CSERE c
ON e.cikkszám = c.cikkszám AND 
e.gyártsz = c.gyártsz
WHERE c.cikkszám IS NULL;





-- Mely cikkekből van csupa visszahozott termék?

SELECT t.cikkszám, t.megnev
FROM CIKK t
JOIN VISSZAFIZ v 
ON t.cikkszám = v.cikkszám
GROUP BY t.cikkszám, t.megnev

HAVING COUNT(DISTINCT v.gyártsz) = 
(SELECT COUNT(*) FROM ELADÁS e WHERE e.cikkszám = t.cikkszám);






-- Mely eladott cikkből volt a legkevesebb csere?

SELECT TOP 1 e.cikkszám, COUNT(c.cikkszám) AS cserélt
FROM ELADÁS e
LEFT JOIN CSERE c
ON e.cikkszám = c.cikkszám AND e.gyártsz = c.gyártsz
GROUP BY e.cikkszám
ORDER BY cserélt ASC;
