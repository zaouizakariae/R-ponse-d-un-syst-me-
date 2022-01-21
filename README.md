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

![Capture d’écran 2022-01-21 104926](https://user-images.githubusercontent.com/8589155•	Le diagramme de Bode en utilisant la commande  bode(G)

- On divise la troisième figure sur 3 en utilisant la commande subplot :
  -  Le diagramme de Bode en utilisant la commande  bode(sys)
  -  Le diagramme de Nyquist à travers la commande Nyquist(sys) 
  -  Le diagramme de Black-Nichols en utilisant la commande : Nychols(sys) 
- Le résultat est le suivant :

![hggg](https://user-images.githubusercontent.com/85891554/150604593-5a379d8c-0cf0-4452-af56-88dcf1f8a7c9.png)

## Influence des pôles et des zéros sur la réponse temporelle 

![Capture d’écran 2022-01-21 225315](https://user-images.githubusercontent.com/85891554/150605200-2d8c94b7-16c3-4b49-b2b7-a3a3d503260e.png)
avec : k=1 , z1= 15 et z2=5

- On génére les  différents fonctions de transfert en utilisant zpk(zeros,poles,gain) 

```matlab
H1=zpk(1/10,[-1/15 -1/5],1)
H2=zpk([],[-1/15 -1/5],1)
H3=zpk(-1/10,[-1/15 -1/5],1)
H4=zpk(-1/15,[-1/15 -1/5],1)
H5=zpk(1/50,[-1/15 -1/5],1)
```

- Pour chaque fonction de transfert on trace la réponse à un échelon unité a l aide de la commande step(Hi) dans le même graphe :

![Capture d’écran 2022-01-21 225605](https://user-images.githubusercontent.com/85891554/150605494-9fcea5c9-488d-4a63-8a40-41e39584e8ce.png)

- On crée un programme qui tracera dans le même graphique la réponse indicielle de la fonction de transfert suivante :

![Capture d’écran 2022-01-21 225907](https://user-images.githubusercontent.com/85891554/150605798-dc3412ec-ff7d-4a2b-b3dc-18bdd2182685.png)

- Dans ce programme on a utilisé un boucle for pour créer les 8 fonctions de transferts correspondants d’une manière automatique. Ensuite pour les générer,on a choisit comme outil la commande :
  - zpk([],[-k-5i -k+5i], abs((k+5i)^2)) :Zéro-pole-gain model qui sont une représentation des fonctions de transfert sous forme factorisée
- après on applique la fonction step(sys) pour tracer la réponse indicielle :

```matlab
leg=[]
for k=1:8
    sys=zpk([],[-k-5i -k+5i], abs((k+5i)^2))
    step(sys)
    hold all
    lines=horzcat('poles=',num2str(k),'+-5','i')
    leg = strvcat(leg,lines)
    legend(leg)
end
```

- On obtient le graphe suivant :

![Capture d’écran 2022-01-21 230606](https://user-images.githubusercontent.com/85891554/150606470-ae8c284b-b7c5-4f01-bda8-0c8fcdb65e86.png)

## Etude d’un actionneur électromécanique

![ya](https://user-images.githubusercontent.com/85891554/150606621-96ecb1e0-f968-498b-9d10-9e8df08c57fc.png)

![fff](https://user-images.githubusercontent.com/85891554/150606627-3552dba3-6c9d-46d9-a19e-c95db55d5bda.png)

- Sans frottement :
  - La génération de la fonction de transfert :
  ```matlab
  sys= tf(beta, [L*M (R*M+L*f) (R*f+L*K+alpha*beta) R*K])
  ```
  - Command window:
  ```matlab
  sys =
 
                  6
  ---------------------------------
  300 s^3 + 300 s^2 + 151.2 s + 150

  ```
  - Calcul du gain statique: 
  ```matlab
  g= dcgain(sys)
  ```
  - Command window:
  ```matlab
  g =
          0.0400
  ```
  - Les pôles et les zéros:
  ```matlab
  z=zero(sys)
  p=pole(sys)
  ```
  - Command window:
  ```matlab
  % Il y a pas de zéros dans ce système car le numérateur est une constante
  z = 0×1 empty double column vector
  
  p =  -0.9973 + 0.0000i
       -0.0013 + 0.7081i
       -0.0013 - 0.7081i
  ```
  ```matlab
  pzmap(S);
  grid on;
  ```  
  
  ![Capture d’écran 2022-01-21 232612](https://user-images.githubusercontent.com/85891554/150608399-4e7de9d2-6cdf-42b3-b00b-f5bb21c098c6.png)
  
  - on Trace la réponse y(t) lorsqu’on envoie une impulsion de tension au système :
  
  ```matlab
  yimp=impulse(S);
  figure(7)
  subplot(2,1,1);
  plot(yimp);
  grid on;
  ```
  - on trace la réponse y(t) lorsqu’on applique un échelon de tension u = 100 V au système. :
  ```matlab
  yech=step(S);
  yx=[0:1:2500];
  ysim=lsim(S,100*ones(size(yx)),yx);
  subplot(2,1,2);
  plot(yx,ysim);
  hold on
  plot(yx,100*ones(size(yx))*GAIN);
  grid on
  ```
  
  ![Capture d’écran 2022-01-21 234447](https://user-images.githubusercontent.com/85891554/150609926-758a5995-c476-4afa-8314-6ce5f481884c.png)

  - Comment pouvait-on prévoir la valeur de régime permanent ?
    - c’est celle du gain*amplitude de l’échelon appliqué dans ce cas : y_inf = 4.
  - Quel est le type de régime transitoire dans ce cas ?
    - On a un régime transitoire pseudo-périodique
  - Quel est approximativement le temps de réponse de ce système ?
    ```matlab
    plot(yx,100*ones(size(yx))*GAIN); 
    grid on
    hold on;
    plot(yx,100*ones(size(yx))*GAIN*0.95);
    hold on;
    plot(yx,100*ones(size(yx))*GAIN*1.05);
    ```
    
    ![Capture d’écran 2022-01-21 234758](https://user-images.githubusercontent.com/85891554/150610193-6b5c39de-af4f-41ba-9704-943932b2df5a.png)
    
```matlab
%On améliore le modèle en prenant en compte le frottement 
visqueux (f ? 0)
F=15;
S=tf([B],[L*M (R*M+L*F) (R*F+L*K+A*B) R*K]);
%calcule du gain a l'aide du fonction dcgain
GAIN=dcgain(S);
P=pole(S);
Z=zero(S);
figure(8);
pzmap(S);
grid on
yimp=impulse(S);
figure(9)
subplot(2,1,1);
plot(yimp);
grid on;
yech=step(S);
yx=[0:0.1:30];
ysim=lsim(S,100*ones(size(yx)),yx);
subplot(2,1,2);
%plot(yech);
plot(yx,ysim);
hold on
plot(yx,100*ones(size(yx))*GAIN);
grid on
````
   - On remarque que la réponse à échelon dans le cas de f = 0 un signal oscillant d’une manière infinie (conservation d’énergie), tandis que lorsque f ≠ 0 on obtient un signal pseudo périodique amortie vue que f traduit le fait d’amortissement par dissipation d’énergie

![Capture d’écran 2022-01-21 235005](https://user-images.githubusercontent.com/85891554/150610446-c9f7e3c9-bbaf-4658-9b48-aa85a069e49d.png)

![Capture d’écran 2022-01-21 235027](https://user-images.githubusercontent.com/85891554/150610450-b82ba978-3b8f-4656-b1e9-a7f15bcdef35.png)

Sans frottement le régime est pseudo-périodique alors qu’avec frottement le régime est apériodique

# THE END
