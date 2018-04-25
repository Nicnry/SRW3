LAN [intranet] 192.168.106.128 (9 pts.)
Nouvelle navigation anonyme

[ ] http://192.168.106.128 (OK pas d'authentification)
[ ] http://192.168.106.128/internet (OK pas d'authentification)
[ ] http://192.168.106.128/internet/dclient (OK dclient)
Nouvelle navigation anonyme

[ ] http://192.168.106.128/internet/dclient (auth. massain) NOK comptable
[ ] http://192.168.106.128/internet/dclient (auth. jbricot) OK ingénieur
[ ] http://192.168.106.128/users/mdupont (NOK sans nouvelle auth.)
[ ] http://192.168.106.128/users/mdupont (OK auth. mdupont)
[ ] http://192.168.106.128/users/jbricot (NOK sans nouvelle auth.)
[ ] http://192.168.106.128/users/jbricot (OK auth. jbricot)

WAN [internet] 172.17.100.168 (11 pts.)
Nouvelle navigation anonyme

[ ] http://172.17.100.168 (Pas d'authentification)
[ ] http://172.17.100.168/dclient (auth. dclient)
Nouvelle navigation anonyme

[ ] http://172.17.100.168/dclient (auth. jbricot) - OK ingénieur
[ ] http://172.17.100.168/intranet (OK sans nouvelle auth.)
[ ] http://172.17.100.168/intranet/users/jbricot (OK sans nouvelle auth.)
[ ] http://172.17.100.168/intranet/users/massain (NOK sans nouvelle auth.)
[ ] http://172.17.100.168/intranet/users/massain (auth. massain)
Nouvelle navigation anonyme

[ ] http://172.17.100.168/intranet (OK auth. kdiocy)
[ ] http://172.17.100.168/intranet/users/jbricot (NOK sans nouvelle auth.)
[ ] http://172.17.100.168/intranet/users/jbricot (auth jbricot)
[ ] http://172.17.100.168/dclient (OK auth. jbricot)
Divers (1 pts.)
[ ] Utilisation d'un groupe `Ingénieurs`
SSL (4 pts. + 2 pts. bonus)
[ ] https://172.17.100.168
[ ] https://192.168.106.128

[ ] Redirection http --> https (http://[internet]/intranet) - BONUS
Total de points : _____ / 27 pts