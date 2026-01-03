## Per què usem claus públiques/asimètriques
### Limitacions clau simètrica

- `Distribució de claus`, s'han de compartir abans d'utilitzar (de manera segura)
- `(n*(n-1))/2` on n=#n usuaris; claus necessàries per n usuaris dins una xarxa de comunicacions
-  `No repudi`, no se sap qui fa que, tant A com B usen mateixa clau

### Problemes matemàtics difícils

- `Factorització d'enters`:  
	- ex: RSA --> n = p * q es difícil que l'atacant trobi valors p i q que formen n nomès sabent n
- `Logaritme discret`: 
	- (donat y, p, x)
	- trobar x
	- y = g^x mod p a Zp
- `Logaritme disret sobre corbes elíptiques`:
	- Buscar Google

Les limitacions de la clau simètrica porten a la necessitat de les claus asimètriques/públiques que garantítzen una seguretat basada en els problemes matemàtics difícils plantejats anteriorment, fent que un atacant no pugui obtenir-les/desxifrar-les.

## Xifrat

### RSA (determinista)

### El Gamal (probabilistica)

### Diffie-Hellman (distribució de claus)


## Signatures i sobres digitals

### RSA (determinista)

### El Gamal (probabilistica)

## Funcions Hash

## Protocols criptogràfics
