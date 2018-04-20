# Cours SRW3

Henry Nicolas

SI-T1a

## Laboratoire

LABO : POC (proof of concept) LAMP

## Travail pratique

Le but des tests est de vérifier que la réalisation respecte les spécifications de départ. Les tests consistent à vérifier que le logiciel réalise les fonctions attendues, et ce correctement. Les tests consistent à exécuter le logiciel et à tester ces fonctionnalités.

| Nom     | Prénom | Fonction  | Login   | Password     |
|---------|--------|-----------|---------|--------------|
| DUPONT  | Marcel | Directeur | mdupont | Qwertz123456 |
| BRICOT  | Juda   | Ingénieur | jbricot | Asdfgh123456 |
| ASSAIN  | Marc   | Comptable | massain | Yxcvbn123456 |
| DEUF    | John   | Ingénieur | jdeuf   | Qaywsx123456 |
| DIOCY   | Kelly  | Ingénieur | kdiocy  | Edcrfv123456 |
| Clients |        |           | dclient | Tgbzhn123456 |
LAN [intranet] 192.168.XXX.XXX (9 pts.)
Nouvelle navigation anonyme

[ ] http://[intranet] (OK pas d'authentification)
[ ] http://[intranet]/internet (OK pas d'authentification)
[ ] http://[intranet]/internet/dclient (OK dclient)
Nouvelle navigation anonyme

[ ] http://[intranet]/internet/dclient (auth. massain) NOK comptable
[ ] http://[intranet]/internet/dclient (auth. jbricot) OK ingénieur
[ ] http://[intranet]/../mdupont (NOK sans nouvelle auth.)
[ ] http://[intranet]/../mdupont (OK auth. mdupont)
[ ] http://[intranet]/../jbricot (NOK sans nouvelle auth.)
[ ] http://[intranet]/../jbricot (OK auth. jbricot)
WAN [internet] 172.17.XXX.XXX (11 pts.)
Nouvelle navigation anonyme

[ ] http://[internet] (Pas d'authentification)
[ ] http://[internet]/dclient (auth. dclient)
Nouvelle navigation anonyme

[ ] http://[internet]/dclient (auth. jbricot) - OK ingénieur
[ ] http://[internet]/intranet (OK sans nouvelle auth.)
[ ] http://[internet]/intranet/../jbricot (OK sans nouvelle auth.)
[ ] http://[internet]/intranet/../massain (NOK sans nouvelle auth.)
[ ] http://[internet]/intranet/../massain (auth. massain)
Nouvelle navigation anonyme

[ ] http://[internet]/intranet (OK auth. kdiocy)
[ ] http://[internet]/intranet/../jbricot (NOK sans nouvelle auth.)
[ ] http://[internet]/intranet/../jbricot (auth jbricot)
[ ] http://[internet]/dclient (OK auth. jbricot)
Divers (1 pts.)
[ ] Utilisation d'un groupe `Ingénieurs`
SSL (4 pts. + 2 pts. bonus)
[ ] https://[internet]
[ ] https://[intranet]

[ ] Redirection http --> https (http://[internet]/intranet) - BONUS