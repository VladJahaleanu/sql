SELECT sysdate from dual;
SET SERVEROUTPUT ON;

CREATE TABLE MENIU(
meniu_id NUMBER(5) NOT NULL PRIMARY KEY,
specific VARCHAR2(20) NOT NULL
);
describe MENIU;

CREATE TABLE DISTRIBUITOR(
  distribuitor_id NUMBER(5) NOT NULL PRIMARY KEY,
  nume_distribuitor VARCHAR2(20) NOT NULL,
  numar_depozite NUMBER(5) NOT NULL
);
describe DISTRIBUITOR;

CREATE TABLE LOCATIE(
  locatie_id NUMBER(5) NOT NULL PRIMARY KEY,
  meniu_id NUMBER(5) NOT NULL,
  distribuitor_id NUMBER(5) NOT NULL,
  oras VARCHAR2(20) NOT NULL,
  numar_sali NUMBER(5) NOT NULL,
  CONSTRAINT fk_meniu FOREIGN KEY (meniu_id) REFERENCES MENIU(meniu_id),
  CONSTRAINT fk_distrib FOREIGN KEY (distribuitor_id) REFERENCES DISTRIBUITOR(distribuitor_id)
);
DESCRIBE LOCATIE;

CREATE TABLE SALA(
  sala_id NUMBER(5) NOT NULL PRIMARY KEY,
  locatie_id NUMBER(5) NOT NULL,
  nr_mese NUMBER(5) NOT NULL,
  CONSTRAINT fk_locatie FOREIGN KEY (locatie_id) REFERENCES LOCATIE(locatie_id)
);
describe SALA;

CREATE TABLE MASA(
  masa_id NUMBER(5) NOT NULL PRIMARY KEY,
  sala_id NUMBER(5) NOT NULL,
  capacitate_masa NUMBER(5) NOT NULL,
  CONSTRAINT fk_sala FOREIGN KEY (sala_id) REFERENCES SALA(sala_id)
);
describe MASA;

CREATE TABLE ANGAJAT(
  angajat_id NUMBER(5) NOT NULL PRIMARY KEY,
  locatie_id NUMBER(5) NOT NULL,
  nume VARCHAR2(25) NOT NULL,
  prenume VARCHAR2(25) NOT NULL,
  email VARCHAR2(50),
  telefon VARCHAR2(10) NOT NULL,
  salariu NUMBER(7) NOT NULL,
  data_angajarii DATE DEFAULT sysdate,
  CONSTRAINT fk_locatie2 FOREIGN KEY (locatie_id) REFERENCES LOCATIE(locatie_id)
);
describe ANGAJAT;

CREATE TABLE MANAGER(
  angajat_id NUMBER(5) NOT NULL PRIMARY KEY,
  CONSTRAINT fk_angajat FOREIGN KEY (angajat_id) REFERENCES ANGAJAT(angajat_id),
  studii VARCHAR2(20)
);
describe MANAGER;

CREATE TABLE BUCATAR(
  angajat_id NUMBER(5) NOT NULL PRIMARY KEY,
  CONSTRAINT fk_angajat1 FOREIGN KEY (angajat_id) REFERENCES ANGAJAT(angajat_id),
  specializare VARCHAR2(20)
);
describe BUCATAR;


CREATE TABLE CHELNER(
  angajat_id NUMBER(5) NOT NULL PRIMARY KEY,
  CONSTRAINT fk_angajat2 FOREIGN KEY (angajat_id) REFERENCES ANGAJAT(angajat_id)
);
describe CHELNER;

CREATE TABLE INGRIJITOR(
  angajat_id NUMBER(5) NOT NULL PRIMARY KEY,
  CONSTRAINT fk_angajat3 FOREIGN KEY (angajat_id) REFERENCES ANGAJAT(angajat_id),
  sala_id NUMBER(5) NOT NULL,
  CONSTRAINT fk_sala2 FOREIGN KEY (sala_id) REFERENCES SALA(sala_id)
);
describe INGRIJITOR;

CREATE TABLE PAZNIC(
  angajat_id NUMBER(5) NOT NULL PRIMARY KEY,
  CONSTRAINT fk_angajat4 FOREIGN KEY (angajat_id) REFERENCES ANGAJAT(angajat_id),
  tura NUMBER(2) NOT NULL
);
describe PAZNIC;

CREATE TABLE LUCREAZA(
  angajat_id NUMBER(5) NOT NULL,
  CONSTRAINT fk_ang FOREIGN KEY (angajat_id) REFERENCES CHELNER(angajat_id),
  sala_id NUMBER(5) NOT NULL,
  CONSTRAINT fk_sala3 FOREIGN KEY (sala_id) REFERENCES SALA(sala_id),
  PRIMARY KEY(angajat_id, sala_id)
);
describe LUCREAZA;

CREATE TABLE COMANDA(
  comanda_id NUMBER(5) NOT NULL PRIMARY KEY,
  masa_id NUMBER(5) NOT NULL,
  valoare NUMBER(5) NOT NULL,
  modalitate_plata VARCHAR2(5) NOT NULL,
  data_comanda DATE DEFAULT sysdate,
  CONSTRAINT fk_masa FOREIGN KEY (masa_id) REFERENCES MASA(masa_id)
);
describe comanda;

commit;

insert into MENIU values(200, 'italian');
insert into MENIU values(201, 'grecesc');
insert into MENIU values(202, 'chinezesc');
insert into MENIU values(203, 'romanesc');
insert into MENIU values(204, 'pescaresc');
insert into MENIU values(205, 'turcesc');

select * from meniu;

insert into DISTRIBUITOR values(300, 'Tarrell', 3);
insert into DISTRIBUITOR values(301, 'Avicola', 5);
insert into DISTRIBUITOR values(302, 'Optimeat', 2);
insert into DISTRIBUITOR values(303, 'Cristim', 7);
insert into DISTRIBUITOR values(304, 'Bonduelle', 1);
insert into DISTRIBUITOR values(305, 'Marisan', 3);
insert into DISTRIBUITOR values(306, 'Fishco', 2);

select * from distribuitor;

commit;

insert into LOCATIE values(100, 200, 300, 'Bucuresti', 2);
insert into LOCATIE values(101, 201, 302, 'Iasi', 1);
insert into LOCATIE values(102, 203, 305, 'Pitesti', 1);
insert into LOCATIE values(103, 204, 306, 'Constanta', 2);
insert into LOCATIE values(104, 205, 300, 'Cluj', 1);
insert into LOCATIE values(105, 202, 304, 'Oradea', 3);

select * from locatie;

insert into ANGAJAT values(1, 100, 'Gica', 'Popescu', 'gpopescu@gmail.com', '0729113517', '7000', to_date('01-01-2000', 'dd-mm-yyyy'));
insert into ANGAJAT values(2, 101, 'Andrei', 'Neagu', 'andneagu@gmail.com', '0759167517', '6000', to_date('03-06-2001', 'dd-mm-yyyy'));
insert into ANGAJAT values(3, 102, 'Florin', 'Potcoveanu', 'fpot@gmail.com', '0735123517', '5500', to_date('28-05-2002', 'dd-mm-yyyy'));
insert into ANGAJAT values(4, 103, 'Radu', 'Alexandru', 'radualex@yahoo.com', '0725884625', '6000', to_date('15-05-2001', 'dd-mm-yyyy'));
insert into ANGAJAT values(5, 104, 'Pop', 'Simone', 'pops@hotmail.com', '0738153517', '5800', to_date('21-01-2004', 'dd-mm-yyyy'));
insert into ANGAJAT values(6, 105, 'Marius', 'Aurel', 'mariusa@yahoo.com', '0755153517', '5300', to_date('12-12-2005', 'dd-mm-yyyy'));
insert into MANAGER values(1, 'licenta');
insert into MANAGER values(2, 'doctorat');
insert into MANAGER values(3, 'licenta');
insert into MANAGER values(4, 'master');
insert into MANAGER values(5, 'licenta');
insert into MANAGER values(6, 'bacalaureat');
select * from angajat;



select * from manager;

insert into ANGAJAT values(7, 100, 'Nicolae', 'Mihai', 'nicmih@gmail.com', '0724567517', '3500', to_date('02-01-2000', 'dd-mm-yyyy'));
insert into ANGAJAT values(8, 101, 'Marian', 'Andrei', 'mandrei@hotmail.com', '0712653517', '3000', to_date('04-06-2001', 'dd-mm-yyyy'));
insert into ANGAJAT values(9, 102, 'Dan', 'Aurica', 'danaurica@gmail.com', '0722133519', '3800', to_date('29-05-2002', 'dd-mm-yyyy'));
insert into ANGAJAT values(10, 103, 'Ion', 'Dragos', 'dragos@yahoo.com', '0758989333', '4000', to_date('16-05-2001', 'dd-mm-yyyy'));
insert into ANGAJAT values(11, 104, 'Popovici', 'Mihai', 'vmp@gmail.com', '0722133514', '3000', to_date('22-01-2004', 'dd-mm-yyyy'));
insert into ANGAJAT values(12, 105, 'Popica', 'Alexandru', 'popica@gmail.com', '0723453519', '3350', to_date('13-12-2005', 'dd-mm-yyyy'));
insert into ANGAJAT values(31, 100, 'Dicu', 'Alexandru', NULL, '0723477755', '3200', to_date('19-12-2006', 'dd-mm-yyyy'));
insert into ANGAJAT values(32, 101, 'Sandu', 'George', NULL, '0733478854', '3150', to_date('25-01-2009', 'dd-mm-yyyy'));
insert into ANGAJAT values(33, 102, 'Norocea', 'Razvan', NULL, '0723227752', '2800', to_date('29-11-2010', 'dd-mm-yyyy'));
insert into ANGAJAT values(34, 103, 'Lupuleac', 'Alexandru', NULL, '0733477733', '3830', to_date('19-12-2006', 'dd-mm-yyyy'));
insert into ANGAJAT values(35, 104, 'Mocanu', 'Horia', NULL, '0755477788', '2200', to_date('28-06-2009', 'dd-mm-yyyy'));
insert into ANGAJAT values(36, 105, 'Stan', 'Ion', NULL, '0766677756', '3200', to_date('12-04-2008', 'dd-mm-yyyy'));
insert into BUCATAR values(7, 'Paste');
insert into BUCATAR values(8, 'Salate');
insert into BUCATAR values(9, 'Desert');
insert into BUCATAR values(10, 'Peste');
insert into BUCATAR values(11, 'Shaorma');
insert into BUCATAR values(12, 'Carne');
insert into BUCATAR values(31, 'Carne');
insert into BUCATAR values(32, 'Carne');
insert into BUCATAR values(33, 'Desert');
insert into BUCATAR values(34, 'Carne');
insert into BUCATAR values(35, 'Salate');
insert into BUCATAR values(36, 'Supe');

select * from bucatar;

insert into ANGAJAT values(13, 100, 'Istrate', 'Andrei', 'adnreiii@gmail.com', '0723455569', '2350', to_date('05-01-2000', 'dd-mm-yyyy'));
insert into ANGAJAT values(14, 101, 'Ionica', 'Teodor', 'ththth@yahoo.com', '0756783519', '2550', to_date('08-06-2001', 'dd-mm-yyyy'));
insert into ANGAJAT values(15, 102, 'Manole', 'Alexandru', 'malone@gmail.com', '0754353512', '3050', to_date('13-12-2002', 'dd-mm-yyyy'));
insert into ANGAJAT values(16, 103, 'Bantescu', 'Matei', 'bantebines@hotmail.com', '0798763516', '3350', to_date('17-05-2001', 'dd-mm-yyyy'));
insert into ANGAJAT values(17, 104, 'Radu', 'Mihnea', 'rmihnea@gmail.com', '0723409870', '2575', to_date('13-02-2004', 'dd-mm-yyyy'));
insert into ANGAJAT values(18, 105, 'Popica', 'Alexandru', 'popica@gmail.com', '0723453519', '3350', to_date('13-12-2005', 'dd-mm-yyyy'));
insert into ANGAJAT values(37, 100, 'Luis', 'Gabriel', NULL, '0762672756', '2970', to_date('12-04-2008', 'dd-mm-yyyy'));
insert into ANGAJAT values(38, 101, 'Bratu', 'Tudor', NULL, '0763673753', '3000', to_date('19-05-2010', 'dd-mm-yyyy'));
insert into CHELNER values(13);
insert into CHELNER values(14);
insert into CHELNER values(15);
insert into CHELNER values(16);
insert into CHELNER values(17);
insert into CHELNER values(18);
insert into CHELNER values(37);
insert into CHELNER values(38);

select * from chelner;

insert into ANGAJAT values(19, 100, 'Voica', 'Andreea', 'voica@gmail.com', '0722131481', '2550', to_date('05-01-2000', 'dd-mm-yyyy'));
insert into ANGAJAT values(20, 101, 'Velea', 'Bogdan', 'velea@gmail.com', '0723455569', '2280', to_date('08-06-2001', 'dd-mm-yyyy'));
insert into ANGAJAT values(21, 102, 'Agigea', 'Mirela', 'agigea@gmail.com', '0723453385', '2100', to_date('13-12-2002', 'dd-mm-yyyy'));
insert into ANGAJAT values(22, 103, 'Gica', 'Aurel', 'aurica@yahoo.com', '0723453671', '2105', to_date('21-05-2001', 'dd-mm-yyyy'));
insert into ANGAJAT values(23, 104, 'Maistru', 'Alexandra', 'maistru@gmail.com', '0723457788', '2230', to_date('13-12-2004', 'dd-mm-yyyy'));
insert into ANGAJAT values(24, 105, 'Ionescu', 'Viorel', 'vio@hotmail.com', '0723455585', '2075', to_date('19-12-2006', 'dd-mm-yyyy'));
insert into INGRIJITOR values(19, 400);
insert into INGRIJITOR values(20, 401);
insert into INGRIJITOR values(21, 402);
insert into INGRIJITOR values(22, 403);
insert into INGRIJITOR values(23, 404);
insert into INGRIJITOR values(24, 405);

select * from ingrijitor;



insert into ANGAJAT values(25, 100, 'Voronet', 'Viorel', 'viorel@gmail.com', '0723451234', '1900', to_date('19-02-2000', 'dd-mm-yyyy'));
insert into ANGAJAT values(26, 101, 'Biban', 'Marius', 'biban@hotmail.com', '0723457585', '2075', to_date('09-06-2001', 'dd-mm-yyyy'));
insert into ANGAJAT values(27, 102, 'Carioveanu', 'Mircea', '', '0723445580', '2115', to_date('19-12-2002', 'dd-mm-yyyy'));
insert into ANGAJAT values(28, 103, 'Birloveanu', 'Stefan', 'birlo@yahoo.com', '0723452282', '1920', to_date('23-05-2001', 'dd-mm-yyyy'));
insert into ANGAJAT values(29, 104, 'Ciortan', 'Alexandru', 'ciortan@hotmail.com', '0723433583', '2225', to_date('02-11-2007', 'dd-mm-yyyy'));
insert into ANGAJAT values(30, 105, 'Serbanescu', 'Costin', 'costin@gmail.com', '0723477787', '2000', to_date('19-12-2006', 'dd-mm-yyyy'));
insert into PAZNIC values(25, 3);
insert into PAZNIC values(26, 1);
insert into PAZNIC values(27, 2);
insert into PAZNIC values(28, 1);
insert into PAZNIC values(29, 3);
insert into PAZNIC values(30, 2);

select * from paznic;
select * from angajat;

insert into SALA values(400, 100, 40);
insert into SALA values(406, 100, 40);
insert into SALA values(401, 101, 15);
insert into SALA values(402, 102, 18);
insert into SALA values(403, 103, 37);
insert into SALA values(407, 103, 30);
insert into SALA values(404, 104, 22);
insert into SALA values(405, 105, 50);
insert into SALA values(408, 105, 20);
insert into SALA values(409, 105, 22);

select * from sala;

insert into MASA values(500, 400, 5);
insert into MASA values(501, 400, 6);
insert into MASA values(502, 400, 2);
insert into MASA values(503, 400, 8);
insert into MASA values(504, 400, 5);
insert into MASA values(505, 401, 5);
insert into MASA values(506, 406, 10);
insert into MASA values(507, 406, 7);
insert into MASA values(508, 401, 7);
insert into MASA values(509, 401, 2);
insert into MASA values(510, 402, 5);
insert into MASA values(511, 402, 10);
insert into MASA values(512, 402, 2);
insert into MASA values(513, 402, 6);
insert into MASA values(514, 403, 5);
insert into MASA values(515, 403, 3);
insert into MASA values(516, 404, 4);
insert into MASA values(517, 404, 5);
insert into MASA values(518, 404, 8);
insert into MASA values(519, 405, 5);
insert into MASA values(520, 405, 4);
insert into MASA values(521, 405, 2);
insert into MASA values(522, 406, 4);
insert into MASA values(523, 407, 2);
insert into MASA values(524, 407, 5);
insert into MASA values(525, 407, 7);
insert into MASA values(526, 407, 4);
insert into MASA values(527, 408, 5);
insert into MASA values(528, 408, 2);
insert into MASA values(529, 408, 8);
insert into MASA values(530, 408, 4);
insert into MASA values(531, 409, 4);
insert into MASA values(532, 409, 2);
insert into MASA values(533, 409, 5);
insert into MASA values(534, 409, 8);
select * from masa;

insert into COMANDA values(1000, 500, 300, 'card', to_date('19-12-2020', 'dd-mm-yyyy'));
insert into COMANDA values(1001, 501, 280, 'cash',to_date(sysdate));
insert into COMANDA values(1002, 502, 120, 'card', to_date('12-06-2018', 'dd-mm-yyyy'));
insert into COMANDA values(1003, 511, 450, 'cash', to_date('09-08-2020', 'dd-mm-yyyy'));
insert into COMANDA values(1004, 512, 315, 'card', to_date('19-12-2020', 'dd-mm-yyyy'));
insert into COMANDA values(1005, 505, 90, 'card', to_date('23-02-2021', 'dd-mm-yyyy'));
insert into COMANDA values(1006, 518, 330, 'card', to_date('13-01-2021', 'dd-mm-yyyy'));
insert into COMANDA values(1007, 528, 197, 'cash', to_date(sysdate));
insert into COMANDA values(1008, 533, 30, 'card', to_date('22-12-2018', 'dd-mm-yyyy'));
insert into COMANDA values(1009, 522, 202, 'card', to_date('16-05-2020', 'dd-mm-yyyy'));
insert into COMANDA values(1010, 511, 320, 'cash', to_date('10-10-2020', 'dd-mm-yyyy'));
select * from comanda;

commit;

SET SERVEROUTPUT ON;

--6. Scrieti o functie care afla numarul angajatilor care au fost angajati in primii X oameni la franciza si lucreaza intr-o locatie cu cel putin Y sali.
CREATE OR REPLACE FUNCTION nr_angajati_by_date(x in number, y in number) return number is rez number(10); 
    TYPE table_angajati IS TABLE OF angajat%ROWTYPE INDEX BY BINARY_INTEGER;
    TYPE v_locatii IS VARRAY(100) OF locatie%ROWTYPE;
    angajati table_angajati;
    locatii v_locatii;
    i binary_integer;
    j binary_integer;

BEGIN
    select *
    bulk collect into locatii
    from( select * from locatie where numar_sali >= y );

    select *
    bulk collect into angajati
    from( select * from angajat order by data_angajarii )
    where rownum <= x;
  
    rez := 0;
    i := angajati.first;
  
  
    while (i is not null) loop
        j := locatii.first;
        while (j is not null) loop
            if angajati(i).locatie_id = locatii(j).locatie_id then
                rez := rez + 1;
            end if;
            j := locatii.next(j);
        end loop;
        i := angajati.next(i);
    end loop;
  
    return rez;
END nr_angajati_by_date;
/

DECLARE 
BEGIN 
  DBMS_OUTPUT.PUT_LINE('Numarul de angajati este '|| nr_angajati_by_date(30, 3));
  DBMS_OUTPUT.PUT_LINE('Numarul de angajati este '|| nr_angajati_by_date(5, 2));
END; 
/

commit;

--7. afisati angajatii fiecarei locatii si salariile acestora
CREATE OR REPLACE PROCEDURE afiseaza_angajati is
  CURSOR locatii IS SELECT locatie_id, oras FROM locatie;
  CURSOR angajati IS SELECT locatie_id, nume, salariu FROM angajat;
  id_locatie locatie.locatie_id%type;
  oras_locatie locatie.oras%type;
  id_angajat angajat.locatie_id%type;
  angajat_nume angajat.nume%type;
  salariu_angajat angajat.salariu%type;
BEGIN
  OPEN locatii;
    LOOP
      FETCH locatii INTO id_locatie, oras_locatie;
      EXIT WHEN locatii%NOTFOUND;
      DBMS_OUTPUT.PUT_LINE ('Locatia cu ID-ul '|| id_locatie || ' din orasul '|| oras_locatie ||' are angajatii:');
      OPEN angajati;
        LOOP
          FETCH angajati INTO id_angajat, angajat_nume, salariu_angajat;
          EXIT WHEN angajati%NOTFOUND;
          IF id_locatie = id_angajat THEN
            DBMS_OUTPUT.PUT_LINE(angajat_nume || ' - '  || salariu_angajat || ' lei');
          END IF;
        END LOOP;
        DBMS_OUTPUT.PUT_LINE('');
      CLOSE angajati;
    END LOOP;
  CLOSE locatii;
END afiseaza_angajati;
/

DECLARE 
BEGIN 
  afiseaza_angajati;
END; 
/

commit;

--8. Afisati telefonul celui mai nou angajat care lucreaza in sala data ca parametru

CREATE OR REPLACE FUNCTION telefon_angajat (id_sala sala.sala_id%TYPE) RETURN VARCHAR IS rez_telefon varchar(20);
BEGIN 
    select a.telefon
    into rez_telefon
    from angajat a, locatie l, sala s
    where a.locatie_id = l.locatie_id 
    and l.locatie_id = s.locatie_id
    and a.data_angajarii =
        (select max(a.data_angajarii)
        from angajat a, locatie l, sala s
        where a.locatie_id = l.locatie_id
        and s.locatie_id = l.locatie_id
        and s.sala_id = id_sala);
    RETURN rez_telefon;
EXCEPTION 
    WHEN NO_DATA_FOUND THEN RAISE_APPLICATION_ERROR(-20000, 'Sala data ca parametru nu exista!');
    WHEN OTHERS THEN RAISE_APPLICATION_ERROR(-20002,'Alta eroare!');
    
END telefon_angajat; 
/
BEGIN 
DBMS_OUTPUT.PUT_LINE('Telefonul celui mai nou angajat din sala data ca parametru este: '|| telefon_angajat(402));
END; 
/
BEGIN 
DBMS_OUTPUT.PUT_LINE('Telefonul celui mai nou angajat din sala data ca parametru este: '|| telefon_angajat(5));
END; 
/


--9. Afisati specificul meniului servit in restaurantul in care a fost facuta comanda data ca parametru
CREATE OR REPLACE PROCEDURE specific_restaurant(id_comanda in comanda.comanda_id%type) is rez_specific meniu.specific%type;
BEGIN
    select m.specific
    into rez_specific
    from meniu m, comanda c, masa ma, sala s, locatie l
    where c.masa_id = ma.masa_id
    and ma.sala_id = s.sala_id
    and s.locatie_id = l.locatie_id
    and l.meniu_id = m.meniu_id
    and c.comanda_id = id_comanda;
    DBMS_OUTPUT.PUT_LINE('Specificul este ' || rez_specific);
EXCEPTION 
    WHEN NO_DATA_FOUND THEN RAISE_APPLICATION_ERROR(-20000, 'Comanda data ca parametru nu exista!');
    WHEN TOO_MANY_ROWS THEN RAISE_APPLICATION_ERROR(-20001, 'Restaurantul in care s-a facut comanda are mai multe meniuri specifice!');
    WHEN OTHERS THEN RAISE_APPLICATION_ERROR(-20002, 'Alta eroare!');
    
END specific_restaurant;
/

DECLARE
BEGIN
  --din baza de date curenta cred ca se poate obtine doar exceptia NO_DATA_FOUND
  specific_restaurant(1001);
  specific_restaurant(100); --no_data_found
END;
/

--10. Realizati un trigger care permite modificarea tabelului COMANDA doar pana la ora 22 (ultima comanda este preluata / modificata pana la ora 22)
CREATE OR REPLACE TRIGGER trigger_modificare_comanda
  BEFORE INSERT OR UPDATE OR DELETE ON comanda
BEGIN 
  IF (to_number(extract(hour from systimestamp)) > 19 ) THEN -- am pus 19 deoarece hour from systimestamp intoarce ora in timezone-ul UTC (ora RO-3)
    RAISE_APPLICATION_ERROR(-20000, 'Tabela COMANDA poate fi modificata doar pana la ora 22!'); 
  END IF; 
END; 
/

insert into COMANDA values(1015, 502, 320, 'cash', to_date('12-12-2021', 'dd-mm-yyyy'));

DROP TRIGGER trigger_modificare_comanda;

commit;

--11. Realizati un trigger care sa nu permita modificarea modalitatii de plata a unei comenzi

CREATE OR REPLACE TRIGGER trigger_modificare_plata
  BEFORE UPDATE OF modalitate_plata ON comanda FOR EACH ROW
BEGIN 
  IF (:OLD.modalitate_plata != :NEW.modalitate_plata) THEN 
    RAISE_APPLICATION_ERROR(-20001, 'Modalitatea de plata a unei comenzi nu poate fi modificata!!'); 
  END IF; 
END; 
/

DROP TRIGGER trigger_modificare_plata;

update comanda
set modalitate_plata = 'cash'
where comanda_id = 1002;

--12. Realizati un trigger care adauga informatii despre actiunile userilor asupra bazei de date intr-un tabel de audit

CREATE TABLE audit_table (utilizator VARCHAR2(50), event VARCHAR2(20), data_event date);

CREATE OR REPLACE TRIGGER trigger_audit
    BEFORE CREATE OR DROP OR ALTER ON SCHEMA
BEGIN 
    INSERT INTO audit_table 
    VALUES (SYS.LOGIN_USER, SYS.SYSEVENT, SYSDATE);
END;
/

create table audit_test2(coloana1 varchar2(20), coloana2 varchar2(20));

select * from audit_table;
commit;


--13. Definiti un pachet care sa contina toate obiectele definite mai sus

CREATE OR REPLACE PACKAGE pachet_jvg AS
    FUNCTION nr_angajati_by_date(x in number, y in number) RETURN NUMBER;
    PROCEDURE afiseaza_angajati;
    FUNCTION telefon_angajat (id_sala sala.sala_id%TYPE) RETURN VARCHAR;
    PROCEDURE specific_restaurant(id_comanda in comanda.comanda_id%type);
END pachet_jvg;
/

CREATE OR REPLACE PACKAGE BODY pachet_jvg AS
    FUNCTION nr_angajati_by_date(x in number, y in number) return number is rez number(10); 
        TYPE table_angajati IS TABLE OF angajat%ROWTYPE INDEX BY BINARY_INTEGER;
        TYPE v_locatii IS VARRAY(100) OF locatie%ROWTYPE;
        angajati table_angajati;
        locatii v_locatii;
        i binary_integer;
        j binary_integer;

    BEGIN
    
        select *
        bulk collect into locatii
        from( select * from locatie where numar_sali >= y );
        
        select *
        bulk collect into angajati
        from( select * from angajat order by data_angajarii )
        where rownum <= x;
  
        rez := 0;
        i := angajati.first;
  
  
        while (i is not null) loop
            j := locatii.first;
            while (j is not null) loop
                if angajati(i).locatie_id = locatii(j).locatie_id then
                    rez := rez + 1;
                end if;
                j := locatii.next(j);
            end loop;
            i := angajati.next(i);
        end loop;
  
        return rez;
    END;

--

    PROCEDURE afiseaza_angajati is
        CURSOR locatii IS SELECT locatie_id, oras FROM locatie;
        CURSOR angajati IS SELECT locatie_id, nume, salariu FROM angajat;
        id_locatie locatie.locatie_id%type;
        oras_locatie locatie.oras%type;
        id_angajat angajat.locatie_id%type;
        nume_angajat angajat.nume%type;
        salariu_angajat angajat.salariu%type;
    BEGIN
        OPEN locatii;
            LOOP
                FETCH locatii INTO id_locatie, oras_locatie;
                EXIT WHEN locatii%NOTFOUND;
                DBMS_OUTPUT.PUT_LINE ('Locatia cu ID-ul '|| id_locatie || ' din orasul '|| oras_locatie ||' are angajatii:');
                OPEN angajati;
                    LOOP
                        FETCH angajati INTO id_angajat, nume_angajat, salariu_angajat;
                        EXIT WHEN angajati%NOTFOUND;
                        IF id_locatie = id_angajat THEN
                            DBMS_OUTPUT.PUT_LINE(nume_angajat || ' - '  || salariu_angajat || ' lei');
                        END IF;
                    END LOOP;
                    DBMS_OUTPUT.PUT_LINE('');
                CLOSE angajati;
            END LOOP;
        CLOSE locatii;
    END;
    
--

    FUNCTION telefon_angajat (id_sala sala.sala_id%TYPE) RETURN VARCHAR IS rez_telefon varchar(20);
    BEGIN 
        select a.telefon
        into rez_telefon
        from angajat a, locatie l, sala s
        where a.locatie_id = l.locatie_id 
        and l.locatie_id = s.locatie_id
        and a.data_angajarii =
            (select max(a.data_angajarii)
            from angajat a, locatie l, sala s
            where a.locatie_id = l.locatie_id
            and s.locatie_id = l.locatie_id
            and s.sala_id = id_sala);
        RETURN rez_telefon;
    EXCEPTION 
        WHEN NO_DATA_FOUND THEN RAISE_APPLICATION_ERROR(-20000, 'Sala data ca parametru nu exista!');
        WHEN OTHERS THEN RAISE_APPLICATION_ERROR(-20002,'Alta eroare!');
    
    END;
    
--

    PROCEDURE specific_restaurant(id_comanda in comanda.comanda_id%type) is rez_specific meniu.specific%type;
    BEGIN
        select m.specific
        into rez_specific
        from meniu m, comanda c, masa ma, sala s, locatie l
        where c.masa_id = ma.masa_id
        and ma.sala_id = s.sala_id
        and s.locatie_id = l.locatie_id
        and l.meniu_id = m.meniu_id
        and c.comanda_id = id_comanda;
        DBMS_OUTPUT.PUT_LINE('Specificul este ' || rez_specific);
    EXCEPTION 
        WHEN NO_DATA_FOUND THEN RAISE_APPLICATION_ERROR(-20000, 'Comanda data ca parametru nu exista!');
        WHEN TOO_MANY_ROWS THEN RAISE_APPLICATION_ERROR(-20001, 'Restaurantul in care s-a facut comanda are mai multe meniuri specifice!');
        WHEN OTHERS THEN RAISE_APPLICATION_ERROR(-20002, 'Alta eroare!');
    
    END;
END pachet_jvg;
/

BEGIN
    DBMS_OUTPUT.PUT_LINE('Numarul de angajati este '|| pachet_jvg.nr_angajati_by_date(30, 3));
    pachet_jvg.specific_restaurant(1001);
    DBMS_OUTPUT.PUT_LINE('Telefonul celui mai nou angajat din sala data ca parametru este: '|| pachet_jvg.telefon_angajat(402));
    pachet_jvg.afiseaza_angajati;
END;
/

commit;


