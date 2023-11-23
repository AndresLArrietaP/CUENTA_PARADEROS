%HECHOS
paradero(carabayllo).
paradero(trapiche).
paradero(pro).
paradero(caqueta).
paradero(dosdemayo).
paradero(unmsm).

antes(carabayllo,trapiche).
antes(trapiche,pro).
antes(pro,caqueta).
antes(caqueta,dosdemayo).
antes(dosdemayo,unmsm).

sube(carabayllo,maria).
sube(carabayllo,jesica).
sube(carabayllo,juan).
sube(trapiche,pedro).
sube(trapiche,pablo).
sube(pro,ana).
sube(dosdemayo,jose).

baja(trapiche,juan).
baja(pro,jesica).
baja(caqueta,pedro).
baja(dosdemayo,pablo).

%REGLAS
despues(X,Y) :-  antes(Y,X).

% Lista completa de paraderos ANTES de la estacion Y
aantes(Y, Lista):-
    antes(Lista,Y).
aantes(Y, Lista):-
    antes(Z,Y),aantes(Z, Lista).

% Lista completa de paraderos DESPUES de la estacion Y


ddespues(Y, Lista):-
    despues(Lista,Y).
ddespues(Y, Lista):-
    despues(Z,Y),ddespues(Z, Lista).

% Quien o quienes se bajan DESPUES, solo en la siguiente estacion, de la estacion Y
subdes(Y, ListaPersonas) :-
    findall(Persona, (despues(X, Y), sube(X, Persona)), ListaPersonas).

% Quien o quienes se bajan DESPUES, en todas las estaciones siguientes, de la estacion Y
subddes(Y, ListaPersonas) :-
    findall(Persona, (ddespues(Y, X), sube(X, Persona)), ListaPersonas).

% Quien o quienes se bajan ANTES de la estacion Y
bajante(Y, ListaPersonas) :-
    findall(Persona, (antes(X, Y), baja(X, Persona)), ListaPersonas).

% Quien o quienes se bajan ANTES, en todas las estaciones anteriores, de la estacion Y
bajaante(Y, ListaPersonas) :-
    findall(Persona, (aantes(Y,X), baja(X, Persona)), ListaPersonas).

% Quien o quienes suben DESPUES de la estacion Y
subante(Y, ListaPersonas) :-
    findall(Persona, (antes(X, Y), sube(X, Persona)), ListaPersonas).

% Quien o quienes suben ANTES de la estacion Y
subaante(Y, ListaPersonas) :-
    findall(Persona, (aantes(Y, X), sube(X, Persona)), ListaPersonas).

% Quien o quienes bajan solo DESPUES de la estacion Y
bajdes(Y, ListaPersonas) :-
    findall(Persona, (despues(X, Y), baja(X, Persona)), ListaPersonas).

% Quien o quienes bajan DESPUES de la estacion Y
bajddes(Y, ListaPersonas) :-
    findall(Persona, (ddespues(Y, X), baja(X, Persona)), ListaPersonas).

contar(Lista, N) :-
    length(Lista, N).

cantidadsubdes(Lista,N) :- subdes(Lista,Y),contar(Y, N).
cantidadsubddes(Lista,N) :- subddes(Lista,Y),contar(Y, N).
cantidadbajante(Lista,N) :- bajante(Lista,Y),contar(Y, N).
cantidadbajaante(Lista,N) :- bajaante(Lista,Y),contar(Y, N).
cantidadsubante(Lista,N) :- subante(Lista,Y),contar(Y, N).
cantidadsubaante(Lista,N) :- subaante(Lista,Y),contar(Y, N).
cantidadbajdes(Lista,N) :- bajdes(Lista,Y),contar(Y, N).
cantidadbajddes(Lista,N) :- bajddes(Lista,Y),contar(Y, N).

% Regla para obtener las personas que quedan en un paradero dado
quedan_en_paradero(Personas, Paradero) :-
    findall(Persona, (sube(P, Persona), despues(Paradero,P), \+ baja(P, Persona)), Personas).

% Regla específica para obtener las personas que quedan en unmsm
quedan_en_unmsm(Personas) :-
    paradero(unmsm),
    quedan_en_paradero(Personas, unmsm).