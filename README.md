# skolesystem

jeg gjorde dette direkte i cmd...


SELECT * FROM klasse;


  Henter alle kolonner fra tabellen klasse.
  * betyr "alt".
  Greit for testing, men i praksis bør du velge bare kolonnene du trenger.



-- 1.2 Vis alle elever (fornavn, etternavn, epost)
SELECT fornavn, etternavn, epost FROM elev;


  Henter bare fornavn, etternavn og epost fra elev.
  Dette er tydeligere og raskere enn SELECT *.
  Vi henter kun det vi faktisk trenger.



-- 1.3 Vis alle fag (kode og navn), sortert på kode
SELECT kode, navn FROM fag ORDER BY kode;


  Henter kode og navn fra fag.
  ORDER BY kode sorterer resultatet alfabetisk.
  Uten ORDER BY kan rekkefølgen være tilfeldig.



-- 1.4 Legg inn en ny elev i 2ITA

SELECT id FROM klasse WHERE navn = '2ITA';

INSERT INTO elev (fornavn, etternavn, epost, klasse_id)
VALUES (
    'Testine',
    'Testesen',
    'testine@skole.no',
    (SELECT id FROM klasse WHERE navn = '2ITA')
);


  INSERT INTO legger inn en ny rad i elev.
  Subspørringen finner riktig klasse-id automatisk.
  Feltet aktiv settes automatisk til 1 (DEFAULT).



-- 1.5 Oppdater en elev
UPDATE elev
SET aktiv = 0
WHERE epost = 'testine@skole.no';


  UPDATE endrer en eksisterende rad.
  WHERE gjør at vi bare endrer riktig elev.
  Uten WHERE ville alle elever blitt oppdatert.



-- 1.6 Slett eleven
DELETE FROM elev
WHERE epost = 'testine@skole.no';


  DELETE fjerner en rad fra tabellen.
  WHERE sørger for at vi bare sletter riktig elev.
  Uten WHERE slettes hele tabellen.




--  NIVÅ 2 – JOIN OG RELASJONER


-- 2.1 Liste alle elever med klassenavnet deres
SELECT e.fornavn,
       e.etternavn,
       k.navn AS klasse
FROM elev e
JOIN klasse k ON k.id = e.klasse_id
ORDER BY k.navn, e.etternavn;


  JOIN kobler sammen elev og klasse.
  ON bestemmer hvordan radene hører sammen.
  Resultatet viser elevens navn og hvilken klasse de går i.



-- 2.2 Vis alle elever i klassen 2ITA
SELECT e.fornavn, e.etternavn, e.epost
FROM elev e
JOIN klasse k ON k.id = e.klasse_id
WHERE k.navn = '2ITA';


  Samme JOIN som over.
  WHERE filtrerer slik at vi bare får elever i 2ITA.
  Vi bruker klassenavn fordi det er lettere å forstå enn id.



-- 2.3 Vis hvilke fag elev med id = 1 tar
SELECT f.kode, f.navn
FROM elev_fag ef
JOIN fag f ON f.id = ef.fag_id
WHERE ef.elev_id = 1;


  elev_fag er en koblingstabell mellom elev og fag.
  JOIN henter fagnavn fra fag-tabellen.
  WHERE viser fagene til én bestemt elev.



-- 2.4 Vis alle lærere og fagene de underviser
SELECT l.navn   AS laerer,
       f.kode,
       f.navn   AS fagnavn
FROM laerer l
JOIN laerer_fag lf ON lf.laerer_id = l.id
JOIN fag f          ON f.id        = lf.fag_id
ORDER BY l.navn, f.kode;


  Dette er en mange-til-mange-relasjon.
  Vi må gjennom koblingstabellen laerer_fag.
  Resultatet viser hvilken lærer som underviser hvilket fag.



-- 2.5 Elev, fag, termin og karakter
SELECT e.fornavn,
       e.etternavn,
       f.kode  AS fagkode,
       t.navn  AS termin,
       k.karakter
FROM karakter k
JOIN elev   e ON e.id = k.elev_id
JOIN fag    f ON f.id = k.fag_id
JOIN termin t ON t.id = k.termin_id
ORDER BY e.etternavn, f.kode, t.navn;


  karakter kobler elev, fag og termin sammen.
  Vi bruker JOIN for å få navn i stedet for id.
  Resultatet viser hvem som fikk hvilken karakter i hvilket fag og termin.







