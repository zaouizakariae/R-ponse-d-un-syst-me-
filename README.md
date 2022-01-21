# Reponse d'un systeme
## Objectifs :

- Appréhender au mieux les concepts théoriques de base de la dynamique des systèmes, par l’utilisation du logiciel de simulation MATLAB/SIMULINK. 
- Analyser l’effet du changement des paramètres d’un système sur son comportement dynamique. Cette analyse sera nécessaire pour la conception d’un système de contrôle commande qui répond aux spécifications souhaitées.
   - Tracé des figures : toutes les figures devront être tracées avec les axes et les légendes des axes appropriés.
   - Travail demandé : un script Matlab commenté contenant le travail réalisé et des commentaires sur ce que vous avez compris et pas compris, ou sur ce qui vous a semblé              intéressant ou pas, bref tout commentaire pertinent sur le TP.

## Réponse temporelle et fréquentielle d’un système d’ordre 3

Considérant un système avec la fonction de transfert suivante :

![Capture d’écran 2022-01-21 101833](https://user-images.githubusercontent.com/85891554/150500643-ebbe2ed1-c577-4a79-bc2b-776600183c11.png)

- On a crée la fonction de transfert G en utilisant la commade:tf qui prend comme paramètres les coefficients de la fonction :

```matlab
numerator = [1 2 1];
denominator = [1 3.8 8.76 5.96];
sys = tf(numerator,denominator)
```

- On calcule les pôles et les zéros de la fonction de transfert en utilisant successivement les commandes 

```matlab
p=pole(sys)
z=zero(sys)
```
  - On obtient les résultats suivants:
```matlab
p =
  -1.4000 + 2.0000i
  -1.4000 - 2.0000i
  -1.0000 + 0.0000i
z =-1
    -1
```
- Ensuite on les trace dans un plan complexe en utilisant la fonction pzmap on obtient un diagramme qui contient les pôles et les zéros de la fonction de transfert 
```matlab
figure(1)
pzmap(sys) 
grid on
```
resultat :

![Capture d’écran 2022-01-21 102818](https://user-images.githubusercontent.com/85891554/150502195-7f79f463-3d9b-494d-adc9-553db14f831f.png)

- On calcule le gain statique de la fonction de transfert en utilisant la commande suivante : 

```matlab
G = dcgain(sys) 
```
on obtient comme resultat:

```matlab
G =  0.1678
```
-  on utilise la commande subplot pour divisée une figure sur 4 et on trace :
-  
   - La réponse impulsionnelle du système en utilisant la commande impulse()
   
   ```matlab
   subplot(2,2,1)
   impulse(sys)
   ```
   
   - La réponse à un échelon unité en utilisant la commande step()
   
   ```matlab
   subplot(2,2,2)
   step(sys)
   ```
   
   - La réponse à un échelon d’amplitude 10   
   
   ```matlab
   subplot(2,2,3)
   t=[0:0.1:5];
   input=10*ones(size(t));
   output=lsim(sys,input,t);
   plot(t,input*G)
   hold on
   plot(t,output)
   ```
   
   - La réponse à un signal y(t) = 2*cos(1.6t), dans l’intervalle [0,10]

   ```matlab
   subplot(2,2,4)
   t1=[0:0.1:10];
   input1=2*cos(1.6*t1);
   plot(t1,input1)
   hold on
   plot(t2,lsim(sys,input1,t1))
   ```
   
- Output :

![Capture d’écran 2022-01-21 104926](https://user-images.githubusercontent.com/85891554/150505487-c5fb5c3c-4890-4ce1-bf7e-c33e89d9e8c3.png)

- On divise la troisième figure sur 3 en utilisant la commande subplot :
