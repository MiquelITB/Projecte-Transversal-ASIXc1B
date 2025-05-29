# Projecte Transversal ASIXc1B


![image](https://github.com/user-attachments/assets/a26d6eee-f454-459c-9aa9-ea5ebe3e5939)


**Autors:** Alberto Trujillo, Miguel Valencia, Jordi Bravo  
**Període:** 14/5/2025 - 31/5/2025  
**Grup:** ASIXc1B

## TAULA DE CONTINGUTS

- [Proposta de CPD](#proposta-de-cpd)
- [Redundància energètica](#redundància-energètica)
- [Sistema d'Alimentació Ininterrompuda (SAI)](#sistema-dalimentació-ininterrompuda-sai)
- [Monitoratge intel·ligent – EcoStruxure IT Expert](#monitoratge-intelligent--ecostruxure-it-expert)
- [Generador de suport](#generador-de-suport)
- [1.5. Sostenibilitat](#15-sostenibilitat)
- [1.6 Implementació del CPD al núvol AWS amb els serveis utilitzats](#1.6_Implementació_del_CPD_al_núvol_AWS_amb_els_serveis_utilitzats)
- [1.7 Investigar i comparar eficiència energètica amb altres proveïdors del núvol](#17-investigar-i-comparar-eficiència-energètica-amb-altres-proveïdors-del-núvol)
- [Implantació dels serveis d'àudio i vídeo](#implantació-dels-serveis-dàudio-i-vídeo)
- [Disseny i implementació d'una base de dades](#disseny-i-implementació-duna-base-de-dades)
- [Model Entitat-Relació](#model-entitat-relació)
- [Restriccions](#restriccions)
- [Sostenibilitat](#sostenibilitat)
- [1. Identificació del recursos emprats](#1-identificació-del-recursos-emprats)
- [CPD al núvol](#cpd-al-núvol)
- [Comparativa amb altres proveïdors](#comparativa-amb-altres-proveïdors)

---

## Proposta de CPD

### 1.1 Ubicació física

#### Situació:
- **Barcelona (Principal):** Planta baixa, zona interior sense finestres visibles: [https://maps.app.goo.gl/r64CqrL13](https://maps.app.goo.gl/r64CqrL13)
- **Wexford (Suport):** Ubicació física de la sala a l'edifici de Wexford: [https://maps.app.goo.gl/ZYduzm2kB7orsTqA7](https://maps.app.goo.gl/ZYduzm2kB7orsTqA7)

#### Climatització i control ambiental
El sistema de climatització implementat segueix el model N+1, assegurant la redundància operativa en cas de fallada d'un component. Es disposa d'un sistema de passadissos freds i calents, que optimitza la gestió del flux d'aire per a una dissipació de calor més eficient. A més, s'han incorporat filtres HEPA, els quals garanteixen la neteja de l'aire reduint la presència de partícules nocives que podrien afectar el maquinari. Les temperatures i nivells d'humitat es mantenen dins de rangs específics per evitar condensació i problemes de sobreescalfament.

#### Identificació i seguretat
La sala en qüestió no presenta cap tipus de senyalització externa evident, una mesura estratègica per dificultar-ne la identificació i reduir el risc d'accés no autoritzat. L'accés està restringit, la qual cosa suggereix mecanismes de control com la verificació biomètrica, targetes d'identificació o codis PIN.

#### Infraestructura de cablejat
El cablejat segueix una distribució optimitzada per facilitar la gestió i el manteniment. El cablejat de dades està disposat sota el terra tècnic per reduir interferències electromagnètiques i assegurar una organització ordenada. El cablejat d'alimentació està ubicat sobre el sostre tècnic, facilitant l'accés per a tasques de manteniment i reduint la congestió en zones operatives.

#### Terra tècnic i sostre tècnic
La infraestructura del CPD incorpora terra tècnic, que permet el pas del cablejat i la circulació de l'aire fred per a una refrigeració més eficient. També disposa d'un sostre tècnic desmuntable, el qual facilita les intervencions tècniques i l'accés als elements superiors de la instal·lació sense comprometre la integritat estructural.

#### Distribució dels racks i funcions dels centres
- **CPD de Barcelona** actua com a centre principal, allotjant els serveis crítics i gestionant l'activitat operativa. Els seus racks contenen els servidors de producció, el Switch Core, el Firewall i el sistema d'alimentació ininterrompuda (SAI), mentre que el segon rack inclou el sistema NAS per a còpies de seguretat, el servidor de monitoratge, el Switch d'accés i una PDU intel·ligent per a la gestió energètica.

- **CPD de Wexford**, com a centre secundari, està dissenyat per oferir redundància i suport operatiu. Els seus racks contenen servidors de backup, Switch Core, Firewall, SAI, NAS per a còpies de seguretat, servidor de monitoratge, Switch d'accés i una PDU intel·ligent, permetent la recuperació davant desastres, la distribució de càrrega per optimitzar el rendiment del sistema i la continuïtat operativa en cas de fallada del centre principal.

### 1.2. Infraestructura IT

#### Servidors
| Ubicació | Número | Model | Finalitat |
|----------|--------|-------|-----------|
| Barcelona | 4 | Dell PowerEdge R750 | producció |
| Barcelona | 1 | Dell PowerEdge R360 | monitoratge |
| Wexford | 2 | AWS r6i.8xlarge | Replicació/Backup |

#### Patch Panels
| Ubicació | Número | Model | Finalitat |
|----------|--------|-------|-----------|
| Barcelona | 4 | Cat6a 48 ports | Distribució i organització de la xarxa |
| Wexford | 2 | Cat6a 48 ports | Gestió del cablejat de xarxa |

#### Switches
| Ubicació | Número | Model | Finalitat |
|----------|--------|-------|-----------|
| Barcelona | 1 | Cisco Catalyst 9500 | Switch core |
| Barcelona | 1 | Aruba 2930F | Accés a nivell d'usuari |
| Wexford | 1 | Cisco Catalyst 9500 | Switch core |
| Wexford | 2 | Aruba 2930F | Accés a nivell d'usuari |

### 1.3. Infraestructura Elèctrica

#### Barcelona

##### Redundància energètica
- Doble línia elèctrica independent amb commutació automàtica mitjançant sistema ATS per garantir alimentació contínua.

##### Sistema d'Alimentació Ininterrompuda (SAI)
- **Model:** APC Symmetra LX (integrat dins l'arquitectura Schneider EcoStruxure Micro Data Center)
- **Capacitat:** 16 kVA escalable fins a 16 kVA amb redundància N+1
- **Tipus de conversió:** Doble conversió en línia per garantir una alimentació estable
- **Autonomia:** 12 hores per a càrregues crítiques (basat en una estimació de 10 kW)
- **Tensió d'entrada:** 208 V / 240 V
- **Tensió de sortida:** Configurable per 120/208 V o 120/240 V
- **Bateries:** Tipus plom-àcid amb una vida útil estimada de 3-5 anys
- **Pes:** Aproximadament 286 kg
- **Dimensions:** 83.6 cm d'altura, 47.2 cm d'amplada i 68.8 cm de profunditat
- **Monitoratge:** Compatible amb la plataforma EcoStruxure IT Expert per a control remot i alertes en temps real

##### Monitoratge intel·ligent – EcoStruxure IT Expert
- Monitoratge en temps real de l'estat dels equips i consum energètic
- Alertes intel·ligents per detectar possibles fallades abans que afectin el sistema
- Gestió remota accessible des de qualsevol dispositiu amb connexió a Internet
- Anàlisi de dades per optimitzar el rendiment i la fiabilitat dels equips
- Integració amb EcoStruxure Asset Advisor per a suport expert 24/7

##### Generador de suport
- **Tipus:** Grup electrògen dièsel amb arrencada automàtica
- **Autonomia:** 72 hores de funcionament continu
- **Sistema de monitoratge:** Integrat amb EcoStruxure IT Expert per a control remot i alertes en temps real

#### Wexford

##### Redundància energètica
- Sistemes elèctrics duplicats per a càrregues crítiques, assegurant operació contínua i minimitzant riscos de fallada

##### Sistema d'Alimentació Ininterrompuda (SAI)
- **Model:** APC Smart-UPS Online
- **Capacitat:** 10 kVA amb banc de bateries ampliat
- **Tipus de conversió:** Doble conversió en línia per a una alimentació estable i segura
- **Autonomia:** 10 hores per a càrregues crítiques (basat en una càrrega màxima de 6 kW)
- **Tensió d'entrada:** 208 V / 240 V
- **Tensió de sortida:** Configurable per 120/208 V o 120/240 V
- **Bateries:** Tipus plom-àcid amb una vida útil estimada de 3-5 anys
- **Pes:** Aproximadament 150 kg (segons configuració)
- **Dimensions:** Variable segons el model i configuració
- **Monitoratge:** Integrat amb EcoStruxure IT Gateway per a supervisió remota

##### Monitoratge intel·ligent – EcoStruxure IT Gateway
- Supervisió remota des de Barcelona per garantir control continu
- Gestió centralitzada de l'estat dels equips i consum energètic
- Alertes automatitzades per detectar i prevenir fallades
- Anàlisi predictiva per optimitzar el rendiment i la fiabilitat dels equips
- Integració amb EcoStruxure Asset Advisor per a suport expert 24/7

##### Generador de suport
- **Tipus:** Grup electrògen dedicat
- **Autonomia:** 48 hores de funcionament continu
- **Sistema d'arrencada:** Arrencada automàtica integrada per una resposta immediata en cas de fallada elèctrica
- **Monitoratge de combustible:** Sistema integrat per garantir operació contínua

### 1.4 Seguretat Física i Lògica

#### Física:
- **Control Accés:** 
  - Barcelona: Lector biomètric, targeta RFID, control auditat
  - Wexford: [Elements de control d'accés]
- **Videovigilància:** 
  - Barcelona: Càmeres IP amb enregistrament continu i monitoratge remot
  - Wexford: [Sistema de videovigilància]
- **Incendis:** 
  - Barcelona: Sistema FM-200, detectors de fum i temperatura
  - Wexford: [Sistemes de prevenció, detecció i extinció]
- **Evacuació:** 
  - Barcelona: Vies clares i senyalitzades, portes amb barra antipànic
  - Wexford: [Vies d'evacuació]

#### Lògica:
- **Accés:** Restricció per autorització (mínim privilegi), LDAP i 2FA per a admins
- **Firewall:** pfSense amb DPI i IPS actiu
- **Monitorització:** Zabbix + Prometheus + Grafana en temps real amb alertes
- **Backups:** Incrementals diaris i snapshots setmanals (12 mesos), NAS RAID 10 (Barcelona) i rèplica a Wexford (rsync, ZFS)
- **RAIDs:** RAID 10 al NAS de backup

#### RRLL: 
[Mesures de prevenció de riscos laborals específiques del CPD]

### 1.5. Sostenibilitat

- **Optimització Energia:** Virtualització, fonts eficients (80 PLUS), apagada automàtica, optimització climatització, monitoratge consum
- **Energia Verda:** [Investigació sobre contractació d'energia renovable i/o instal·lació de plaques solars]
- **Estalvi Cablejat:** Planificació acurada de racks i ús estratègic de patch panels/switches
- **Circulació Aire Natural:** [Investigació sobre la viabilitat de sistemes de ventilació natural assistida]
- **Parada Equips:** Gestió d'energia per a equips de comunicacions amb baixa càrrega
- **Equips Baix Consum:** Selecció d'equips amb certificacions d'eficiència i consideració de tecnologies com NVMe

### 1.6 Implementació del CPD al núvol AWS amb els serveis utilitzats

Per complementar la proposta de CPD físic, s'ha creat una instància EC2 a Amazon Web Services (AWS) amb les següents característiques:

#### Detalls de les instàncies:

##### Server 1
| Paràmetre | Valor |
|-----------|-------|
| Nom | G3 - Server |
| Instance ID | i-0571b65dc865ff636 |
| Estat de la instància | Running |
| Tipus de màquina | t2.medium (2 vCPU, 4 GiB RAM) |
| Zona de disponibilitat | us-east-1a |
| Adreça IPv4 pública | 50.17.173.61 |
| DNS públic | ec2-50-17-173-61.compute-1.amazonaws.com |
| Elastic IP | 50.17.173.61 |
| Plataforma | Linux/UNIX |
| Grup de seguretat | launch-wizard-1 |
| Clau d'accés SSH | vockey |

##### Server 2
| Paràmetre | Valor |
|-----------|-------|
| Nom | G3 - Server2 |
| Instance ID | i-08b5506c5a3e60799 |
| Estat de la instància | Running |
| Tipus de màquina | t2.medium (2 vCPU, 4 GiB RAM) |
| Zona de disponibilitat | us-east-1a |
| Adreça IPv4 pública | 18.206.20.187 |
| DNS públic | ec2-52-91-186-158.compute-1.amazonaws.com |
| Elastic IP | 18.206.20.187 |
| Plataforma | Linux/UNIX |
| Grup de seguretat | launch-wizard-1 |
| Clau d'accés SSH | vockey |

##### Server 3
| Paràmetre | Valor |
|-----------|-------|
| Nom | G3 - Server3 |
| Instance ID | i-0c4a6080211797071 |
| Estat de la instància | Running |
| Tipus de màquina | t2.medium (2 vCPU, 4 GiB RAM) |
| Zona de disponibilitat | us-east-1a |
| Adreça IPv4 pública | 54.165.187.241 |
| DNS públic | ec2-54-165-187-241.compute-1.amazonaws.com |
| Elastic IP | 54.165.187.241 |
| Plataforma | Linux/UNIX |
| Grup de seguretat | launch-wizard-3 |
| Clau d'accés SSH | vockey |

#### Serveis utilitzats

1. **Servei d'àudio:** Icecast2 + ezstream | server 2
2. **Servei de vídeo:** FFmpeg | server 3
3. **Bases de dades:** MySQL | server 1 
4. **Servei Web:** Nginx | server 1 i 2
5. **Cockpit** | server 1
6. **Samba** | server 1
7. **Servei SFTP** | server 1
8. **UFW** | server 1
9. **Fail2ban:** S'implementarà per monitoritzar els intents d'accés no autoritzats i bloquejar les IPs sospitoses. server 1

### 1.7 Investigar i comparar eficiència energètica amb altres proveïdors del núvol

| Proveïdor | Percentatge Energia Renovable Actual | Objectiu de Sostenibilitat | Tecnologies d'Eficiència Energètica | PUE Mitjà Aproximat | Altres Aspectes Rellevants |
|-----------|-------------------------------------|----------------------------|-------------------------------------|-------------------|----------------------------|
| AWS | 65% (projecta 100% el 2030) | 100% energia renovable el 2030 | Sistemes avançats de climatització, ús d'energia verda, optimització en el disseny d'infraestructura | ~1.1 - 1.2 | Gran presència global, sistemes de recuperació d'energia tèrmica |
| Microsoft Azure | 100% actual | Carbon negatiu el 2030 | Refrigeració líquida, dissenys modulares per a eficiència, ús d'energia solar i eòlica | ~1.1 | Fins i tot reciclatge d'aigua a centres de dades, suport a energia verda local |
| Google Cloud | 100% actual | Neutralitat de carboni en totes les operacions | IA per optimitzar refrigeració i consum energètic, sistemes avançats de refrigeració per evaporació i líquida | ~1.08 | Líder en eficiència, primer gran proveïdor en assolir energia 100% renovable des de 2017 |

## Disseny i implementació d'una base de dades

### Model Entitat-Relació

#### Empleat
- DNI (CHAR(n))
- Nom (VARCHAR(m))
- Cognom (VARCHAR(m))
- Adreça (VARCHAR(m))
- Telèfon (VARCHAR(m))
- Codi_Dep (INTEGER)
- Codi_GrupNivell

#### Departament
- Codi_Dep (INTEGER)
- Nom_Dep (VARCHAR(m))
- Telèfon_Dep (VARCHAR(m))

#### Grup-Nivell
- Codi_GrupNivell (VARCHAR(m))
- Salari (anual, €) Decimal (p,s)
- PeriodeProva (Mes) INTEGER
- Vacances (dies) INTEGER

**Relacions:**
- Empleat està assignat a Departament (1,N)
- Cada Empleat té un Grup-Nivell (1,N)

### Restriccions

Ha d'existir com a mínim un empleat de cada Grup-Nivell definit.

| Grup-Nivell | Salari (€) | Període de Prova |
|-------------|------------|------------------|
| A1 | 22.246,16 | 6 |
| B1 | 20.377,72 | 6 |
| C1 | 19.485,91 | 6 |
| C2 | 17.346,87 | 4 |
| C3 | 17.056 | 4 |
| D1 | 16.976 | 4 |
| D2 | 16.816 | 3 |
| D3 | 16.736 | 3 |
| E1 | 16.656 | 3 |
| E2 | 16.576 | 3 |

El període de prova per els tècnics titulats es de 6 mesos i per la resta es de 2 mesos. Mentre que els períodes de vacances son 23 dies laborals per a tothom.



![image](https://github.com/user-attachments/assets/32936c67-556a-4571-88bf-3f5dc8398cff)



## Sostenibilitat

La implementació de mesures de sostenibilitat i eficiència energètica son un pilar fonamental per el desenvolupament de qualsevol projecte actual, i dintre del marc dels ODS, per la nostra responsabilitat i respecte amb el medi ambient.

Per tot això, amb el nostre projecte hem decidit seguir el valor de la companyia i aplicar els criteris d'eficiència energètica i sostenibilitat al disseny i desplegament. Aquesta pràctica inclou l'ús racional dels recursos tecnològics amb una optimització de la capacitat computacional, l'anàlisi del consum energètic associat i la cerca activa d'alternatives que minimitzin l'impacte ambiental com per exemple l'ús de energies i fonts renovables o la tria d'una infraestructura cloud que complexi amb el compromís climàtic.

Per poder calcular la nostra contribució real de les emissions de CO₂, considerem essencial calcular la petjada ecològica del projecte. Amb aquest anàlisis ens permetrà fer una avaluació precisa dels punts amb més impacte, proposar mesures concretes de millora i aplicar aquests canvis antes del desplegament. Com per exemple reduir hores de funcionament innecessàries o la tria de una infraestructura més adient.

Si aplicam aquestes idees en el desenvolupament del projecte, no només serà tècnic, sinó que a més serà ètic i compromès amb el futur del nostre planeta.

## 1. Identificació del recursos emprats

### Rack 1 – Producció

Servidors Dell PowerEdge R750: Servidors d'alt rendiment amb múltiples processadors Xeon, RAM de 64 GB a 256 GB, i discs SSD/HDD en configuració RAID. El consum és d'entre 100-150 W per CPU i de 3-5 W per cada 8 GB de RAM.

Switch Core Cisco Catalyst 9500: Switch d'alt rendiment per gestionar el tràfic de xarxa amb un consum d'entre 300 W i 600 W, depenent de la configuració.

Firewall FortiGate 100F: Dispositiu de seguretat de xarxa amb una CPU dedicada, que consumeix uns 40-50 W.

SAI APC Symmetra LX: Sistema d'alimentació ininterrompuda amb un consum d'entre 1,5 kW i 3 kW, que proporciona estabilitat energètica als dispositius del rack.

| Recurs | nº | W | kWh mensuals |
|--------|----|----|--------------|
| Servidors Dell PowerEdge R750 | 4 | 800 | 376 |
| Switch Core Cisco Catalyst 9500 | 1 | 400 | 288 |
| Firewall FortiGate 100F | 1 | 50 | 36 |
| SAI APC Symmetra LX | 1 | 2500 | 1800 |
| **Total** | | | **2500** |

### Rack 2 – Backup i Serveis

NAS Synology RS4021xs+ amb RAID 10: Emmagatzematge en xarxa amb un consum d'entre 30 W i 100 W, depenent de l'ús i la quantitat de discs. Els discs SSD/HDD consumeixen entre 5-15 W cada un.

Servidor de monitoratge: Servidor dedicat al monitoratge amb un consum d'entre 100 W i 250 W, segons la configuració.

Switch d'accés Aruba: Switch amb un consum d'entre 50 W i 150 W, depenent dels ports i del tràfic gestionat.

PDU intel·ligent Eaton: Unitat de distribució d'energia amb un consum baix d'entre 10 W i 50 W.

| Recurs | nº | W | kWh mensuals |
|--------|----|----|--------------|
| NAS Synology RS4021xs+ amb RAID 10 | 1 | 75 | 54 |
| Servidor de monitoratge | 1 | 175 | 126 |
| Switch d'accés Aruba | 1 | 100 | 72 |
| PDU intel·ligent Eaton | 1 | 30 | 21,6 |
| **Total** | | | **273,6** |

### Implementació al Núvol (AWS)

Instància EC2 t2.medium: Instància en el núvol d'AWS amb 2 vCPUs i 4 GiB de RAM. El consum aproximat és de 30-40 W per la CPU i de 10-15 W per la RAM. L'emmagatzematge en EBS consumeix entre 3-10 W per GB. La capacitat de transferència de dades és de fins a 1 Gbps, depenent de la càrrega de treball.

| Recurs | nº | W | kWh mensuals |
|--------|----|----|--------------|
| Instancia EC2 r6i.8xlarge | 1 | 628 | 452,2 |
| **Total** | | | **452,2** |

### Consum total estimat

El consum de kWh estimats per la infraestructura dels servidors sería aproximadament el següent:

- Luminaria
- Refrigeració dels servidors
- **Total Recursos:** 3225,8 kWh
- **Final:** 3225,8 kWh

### Planta fotovoltaica

La energía que podem utilitzar per poder suministrar el CPD, utilitzant l'accessibilitat solar de Barcelona, vendría una estació de producció fotovoltaica. Dimensionam una planta fotovoltaica amb connexió a la red tenint en compte la capacitat i el consum.

El consum mensual aproxima a uns 3.000 kWh/mes, tenint en compte que la radiació mitjana solar a Barcelona es de 4,5kWh/m²/dia i l'eficiència del sistema de un 75% (tenint en compte les pèrdues per temperatura, cablejat, inversor, brutícia…). A més, el terrat on es planteja la instal·lació de la planta hi ocuparía uns 160m² orientació sur, inclinació disponible i sense ombres. Per lo que podríem instalar i encaixa amb el pressupost seria d'uns 75 panells.

Podem definir la producción estimada amb 1 kW de potencia instalada:
```
1kWp * 4,5h/dia * 30 dies * 0,75 = 101,25kWh/mes
```


![image](https://github.com/user-attachments/assets/00c9b35a-7dbb-4619-8894-1df13851d11d)


La potencia de la instalació sería de:
```
3.000 kWh / 101,25kWh/kWp = 29,63kWp
```

Per tant, a les hores de sol, el nostre CPD estaría en funcionament alimentat en gran part per energía verda autoproduïda i sostenible, generant un impacte mínim sobre el planeta.

### Petjada ecològica

#### 1. Com afecta l'emmagatzematge al consum

Les bateries permeten emmagatzemar excedents solars durant el dia i utilitzar-los durant les hores sense producció fotovoltaica. Això redueix el consum de la xarxa elèctrica i, per tant, les emissions de CO₂.

Amb unes que bateries poden cobrir 50% del consum nocturn, la nova dependència de la xarxa seria:

| Estació | Consum total (kWh/mes) | Cobertura solar | Cobertura de bateries | Consum restant de la xarxa |
|---------|------------------------|-----------------|----------------------|---------------------------|
| Estiu | 4198,8 kWh | 60% | 40% | 0 kWh |
| Primavera | 4198,8 kWh | 50% | 40% | 420 kWh |
| Tardor | 4198,8 kWh | 35% | 40% | 1679 kWh |
| Hivern | 4198,8 kWh | 25% | 40% | 2518 kWh |

#### 2. Petjada de carboni amb bateries

Si el consum restant encara depèn de la xarxa elèctrica (amb un factor d'emissió de 0,4 kg CO₂/kWh), les emissions es redueixen significativament:

| Estació | Consum Restant | Emissions mensuals |
|---------|----------------|-------------------|
| Estiu | 0 kWh | 0 tones CO₂ |
| Primavera | 420 kWh | 0,17 tones CO₂ |
| Tardor | 1679 kWh | 0,67 tones CO₂ |
| Hivern | 2518 kWh | 1,01 tones CO₂ |

**Emissions anuals totals amb bateries i energia solar:**
- Primavera: 0,17 tones CO₂ * 3 mesos = 0,51 tones CO₂
- Tardor: 0,67 tones CO₂ * 3 mesos = 2,01 tones CO₂
- Hivern: 1,01 tones CO₂ *
- Total anual: 5,55 tones CO₂/any

## 3. Conclusions

Amb la integració de bateries d'emmagatzematge, la petjada de carboni es redueix d'11,58 tones CO₂/any a només 5,55 tones CO₂/any, representant una reducció de **52,1%** respecte a un sistema sense energia solar.

---

# CPD al núvol

Part de la infraestructura del nostre CPD estarà situada al núvol perquè és una de les plataformes més consolidades, escalables i segures del mercat. Hi ha molts de proveïdors de serveis al núvol que el compararem per saber quin s'adapta millor al que necessitem i qui ofereix un server més segur, estable i configurable. 

Però de tots, hem elegit **AWS (Amazon Web Services)** ja que ofereix una àmplia gamma de serveis que permeten desplegar infraestructures flexibles i d'alta disponibilitat, adaptant-se tant a projectes petits com a entorns empresarials complexos. També disposa de molts d'anys de experiència la qual cosa faciliten les eines d'automatització la compatibilitat amb arquitectures híbrides i la gestió de recursos tecnològics.

## Comparativa amb altres proveïdors

| Característica | AWS | Microsoft Azure | Google Cloud Platform (GCP) | Oracle Cloud |
|----------------|-----|-----------------|------------------------------|--------------|
| **Quota de mercat** | Lideratge global (>30%) | Segon lloc global (~25%) | Tercer lloc (~10%) | Petita quota però creixent (~4%) |
| **Fiabilitat i escalabilitat** | Excel·lent | Excel·lent | Molt bona | Bona |
| **Regions globals** | 30+ regions | 60+ regions | 35+ regions | 40+ regions |
| **Suport a arquitectures híbrides** | Alt (amb AWS Outposts) | Molt alt (Azure Stack) | Mig (Anthos per Kubernetes) | Mig-alt |
| **Facilitat d'ús** | Bona, però més tècnica | Molt bona per a usuaris Windows | Molt bona per desenvolupadors | Bona per a usuaris Oracle |
| **Preus i facturació** | Competitius, per ús real | Similar a AWS | Ligerament més econòmic | Agressiva en preus |
| **Base d'usuaris ideal** | Grans empreses, startups | Empreses que ja usen Microsoft | Desenvolupadors, IA i Big Data | Empreses amb infraestructures Oracle |
| **Suport a IA/ML** | Amplis serveis (SageMaker, etc.) | Bons serveis (Azure ML) | Excel·lent (Vertex AI, TPU) | Limitat |
| **Compatibilitat amb bases de dades** | Alta varietat (RDS, DynamoDB...) | Molt bona (SQL Server, CosmosDB) | Alta (BigQuery, Firestore...) | Excel·lent per a Oracle DB |
| **Seguretat i compliment** | Normatives internacionals completes | Molt bona | Molt bona | Bona |

## Conclusió

Després de fer una comparativa entre els proveïdors més utilitzats, **AWS** és el que ofereix un entorn més complet, tècnic i madur, capaç d'oferir-nos les eines necessàries per poder desenvolupar el nostre projecte de manera segura, eficient i completa.

---
