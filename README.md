# **💧 Tatouage d’Images Chiffrées Homomorphiquement**

Ce projet a été dans le cadre du cours **Cryptologie Avancée et Protection des données de la TAF Cyber de IMT Atlantique à Rennes \- 2025**. Il explore la faisabilité d’insérer un tatouage dans une image, directement dans un **domaine chiffré**. Pour ce faire, nous utilisons le **chiffrement homomorphe de Paillier** couplé à l’algorithme de tatouage **QIM (Quantization Index Modulation)**.

---

## **📝 Objectif et Contexte**

L’objectif principal de ce projet est de démontrer que l’on peut insérer et extraire une marque dans une image, tout en préservant la confidentialité des données. La méthode proposée permet d’effectuer le tatouage à la fois dans le domaine clair et dans le domaine chiffré, garantissant ainsi que la modification apportée soit invisible à l’œil nu dans l’image originale tout en restant détectable lors de l’extraction.

Le projet s’appuie sur deux concepts fondamentaux :

* **Chiffrement Homomorphe de Paillier**  
   Permettant de réaliser des opérations (addition, multiplication par un scalaire) sur des données chiffrées sans déchiffrement intermédiaire. Cela assure la confidentialité tout au long du processus.

* **Quantization Index Modulation (QIM)**  
   Méthode de tatouage numérique qui modifie légèrement les valeurs d’un message (ici, les pixels d’une image) selon un pas de quantification ∆ pour y insérer une marque.  
   Les équations principales utilisées sont :

  * **Insertion :**

  **\!\[Formule Quantification Insertion\](https://github.com/Acuzou/Tatouage-Image-Chiffrement-Homomorphe/tree/main/images/FormuleQuantization.png)**

  * **Extraction :**

**\!\[Formule Quantification Extraction\](https://github.com/Acuzou/Tatouage-Image-Chiffrement-Homomorphe/tree/main/images/FormuleQuantization2.png)**

Ces opérations permettent de tatouer l’image de manière robuste et d’extraire la marque même après chiffrement.

---

## **📁 Contenu du Projet**

* **Notebook Python (`Chiffrement_Homomorphe.ipynb`)**  
   Ce notebook contient l’implémentation complète de la démonstration, de la préparation de l’image à l’extraction de la marque. Il intègre également la gestion du chiffrement/déchiffrement ainsi que les opérations de tatouage dans les deux domaines (clair et chiffré).

* **Document de Référence (`Projet_HE_WAT_2024-1.pdf`)**  
   Le cahier des charges détaille la méthode utilisée, les équations théoriques (QIM), l’algorithme d’insertion dans le domaine chiffré, ainsi que les étapes d’extraction. Il présente également le contexte scientifique et les références bibliographiques de la solution proposée.

* **Code Source**  
   L’ensemble du code nécessaire est intégré dans le notebook, permettant ainsi de reproduire la démonstration sans nécessiter d’intervention supplémentaire.

---

## **🔍 Pipeline de la Démonstration**

Le processus de tatouage se divise en plusieurs étapes :

### **1\. Prétraitement de l'Image**

* **Conversion et Flatten** :  
   L’image est convertie en un vecteur (flatten) pour faciliter le traitement.

* **Découpage en Blocs** :  
   Le vecteur est divisé en blocs de taille égale à celle de la marque à insérer.

* **Insertion de la Pré-Marque** :  
   Pour chaque bloc, une pré-marque est insérée via la modulation QIM avec un pas de quantification (∆, par exemple ∆ \= 1). Cette étape facilite les phases d’insertion et d’extraction de la marque.

### **2\. Chiffrement Homomorphe**

Chaque composant du vecteur pré-marqué est chiffré individuellement à l’aide du cryptosystème de Paillier. L’utilisation d’un aléa différent pour chaque composante renforce la sécurité du chiffrement.

### **3\. Insertion de la Marque**

L’insertion de la marque se réalise en deux étapes distinctes :

#### **3.1 Domaine Clair (ins1)**

* **Principe** :  
   La marque est ajoutée en combinant l’image pré-marquée avec le chiffrement de chaque bit de la marque.

* **Opération Homomorphe** :  
   Pour le jème composant du ième bloc, on effectue l’opération :  
  **\!\[Insertion Domaine Chiffré Ins2\](https://github.com/Acuzou/Tatouage-Image-Chiffrement-Homomorphe/tree/main/images/InsertionDomaineEnClair.png)**  
    
   Cette opération permet d’ajouter la marque dans le domaine clair tout en conservant le chiffrement.

#### **3.2 Domaine Chiffré (ins2)**

* **Méthode Probabiliste** :  
   L’insertion dans le domaine chiffré ajuste l’aléa du chiffrement.  
   En modifiant l’aléa par un facteur aléatoire (multiplication par un chiffrement de 0), le bit inséré est contrôlé pour que, lors d’un modulo 2, il corresponde au bit de la marque.

* **Algorithme** :  
   Tant que le résultat modulo 2 n’est pas égal au bit voulu, l’aléa est modifié de manière itérative.  
    
  **\!\[Insertion Domaine Chiffré Ins1\](https://github.com/Acuzou/Tatouage-Image-Chiffrement-Homomorphe/tree/main/images/InsertionDomaineChiffré.png)**

### **4\. Extraction de la Marque**

L’extraction se fait dans les deux domaines :

#### **4.1 Extraction dans le Domaine Chiffré**

* Un simple **modulo 2** sur chaque composante chiffrée permet de récupérer directement le bit inséré.

  **\!\[Extraction Domaine Chiffré\](https://github.com/Acuzou/Tatouage-Image-Chiffrement-Homomorphe/tree/main/images/ExtractionDomaineChiffré.png)**

#### **4.2 Extraction dans le Domaine Clair**

* **QIM Inverse** :  
   Après déchiffrement, une division par ∆ permet d’obtenir un premier résultat.  
  **\!\[Extraction Domaine En clair 1\](https://github.com/Acuzou/Tatouage-Image-Chiffrement-Homomorphe/tree/main/images/ExtractionEnClair1.png)**  
* **Opération XOR** :  
   Ce résultat est ensuite combiné avec la pré-marque via un XOR pour extraire la marque finale.

  **\!\[Extraction Domaine En clair 2\](https://github.com/Acuzou/Tatouage-Image-Chiffrement-Homomorphe/tree/main/images/ExtractionEnClair2.png)**

Le notebook vérifie systématiquement que la marque extraite correspond à la marque initialement insérée, garantissant ainsi la robustesse de la méthode.

**\!\[Schéma global\](https://github.com/Acuzou/Tatouage-Image-Chiffrement-Homomorphe/tree/main/images/SchemaGlobal.png)**

---

## **🚀 Instructions d’Exécution**

Pour reproduire la démonstration :

1. **Prérequis**  
    Assurez-vous d’avoir installé Python 3 et les bibliothèques suivantes :

   * `gmpy`  
   * `matplotlib`   
   * `numpy`  
   * `tqdm`  
   * (Éventuellement d’autres dépendances pour la gestion du chiffrement homomorphe)

2. **Exécution du Notebook**

   * Ouvrez le fichier `Chiffrement_Homomorphe.ipynb` dans votre environnement Jupyter Notebook ou Google Colab.

   * Exécutez les cellules dans l’ordre pour initialiser le pipeline complet, depuis le prétraitement de l’image jusqu’à l’extraction de la marque.

3. **Vérification**

   * Le notebook affiche l’image tatouée ainsi que la marque extraite pour validation.

   * Il est également vérifié que l’insertion dans le domaine chiffré est invisible dans le domaine clair, assurant la confidentialité.

---

## **🔗 Références**

1. **Bouslimi, D., Bellafqira, R., & Coatrieux, G.**  
    *Data hiding in homomorphically encrypted medical images for verifying their reliability in both encrypted and spatial domains*. In EMBC 2016, IEEE, 2016\.

2. **Chen, B. & Wornell, G. W.**  
    *Quantization Index Modulation: A Class of Provably Good Methods for Digital Watermarking and Information Embedding.* IEEE Transactions on Information Theory, 2001\.

3. **Paillier, P.**  
    *Public-key Cryptosystems Based on Composite Degree Residuosity Classes.* In EUROCRYPT 1999, Springer, 1999\.

---

## **👤 Auteurs**

* **ALBAREDE Khéo**  
  * Etudiant Ingénieur en Cybersécurité \- IMT Atlantique   
* **CUZOU Alexandre**  
  * Etudiant Ingénieur en Cybersécurité \- IMT Atlantique 

---

