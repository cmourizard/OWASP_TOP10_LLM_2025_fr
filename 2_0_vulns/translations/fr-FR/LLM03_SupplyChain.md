## LLM03:2025 Chaîne d’approvisionnement

### Description

Les chaînes d’approvisionnement des LLM sont vulnérables à divers types d’attaques pouvant compromettre l’intégrité des données d’apprentissage, des modèles eux-mêmes et des plateformes de déploiement. Ces risques peuvent entraîner des sorties biaisées, des violations de sécurité ou des défaillances système. Contrairement aux vulnérabilités logicielles classiques (liées au code ou aux dépendances), les risques en apprentissage automatique incluent aussi les modèles pré-entraînés tiers et les données.

Ces éléments externes peuvent être manipulés via des attaques de falsification ou d’empoisonnement.

La création de LLM est une tâche spécialisée qui dépend souvent de modèles tiers. L’essor des LLM en accès libre et de nouvelles méthodes d’ajustement  comme LoRA (Low-Rank Adaptation) et PEFT (Parameter-Efficient Fine-Tuning), notamment sur des plateformes comme Hugging Face, introduit de nouveaux risques de type chaîne d’approvisionnement. De plus, l’apparition des LLM embarqués augmente la surface d’attaque et les risques de type chaîne d’approvisionnement des applications LLM.

Certains risques évoqués ici sont également abordés dans la section "LLM04: Empoisonnement des données et des modèles". Cette section se concentre sur l’aspect "chaîne d’approvisionnement" de ces risques. Un modèle de menace de ce type est disponible [ici](https://github.com/jsotiro/ThreatModels/blob/main/LLM%20Threats-LLM%20Supply%20Chain.png).

### Exemples de risques courants

#### 1. Vulnérabilités des packages tiers

Des composants obsolètes ou dépréciés peuvent être exploités pour compromettre les applications LLM. Cela se rapproche du risque "A06:2021 – Composants vulnérables et obsolètes", amplifié lorsque les composants sont utilisés dans l’apprentissage ou l’ajustement des modèles. (Lien de référence: \[A06:2021 – Composants vulnérables et obsolètes]\([https://owasp.org/Top10/fr/A06\_2021-Vulnerable\_and\_Outdated\_Components/](https://owasp.org/Top10/fr/A06_2021-Vulnerable_and_Outdated_Components/)))

#### 2. Risques liés aux licences

Le développement en IA implique diverses licences liées aux logiciels et aux jeux de données utilisés, cela peut créer des risques en cas de mauvaises gestion de celles-ci. Différentes licences open-sources ou propriétaires imposent divers pre-requis légaux. Les licences des jeux de données peuvent restreindre leur usage, distribution ou commercialisation.

#### 3. Modèles obsolètes ou non maintenus

L’utilisation de modèles obsolètes ou non maintenus expose à des problèmes de sécurité.

#### 4. Modèles pré-entraînés vulnérables

Contrairement aux logiciels libres, les modèles sont des boîtes noires binaires, l'inspection statique n'offre que peu de garanties en matière de sécurité dans ce cas. Un modèle pré-entrainé vulnérable peut contenir des biais cachés, des portes dérobées ou des fonctionnalités malveillantes non identifiées lors des évaluations de sécurité. Ces modèles vulnérables peuvent provenir de jeux de données empoisonnés ou directement d’altération de modèle via des technique comme ROME aussi connue sous le nom de lobotomisation.

#### 5. Provenance des modèles faibles

Il n’existe actuellement pas de garantie forte sur la provenance des modèles publiés. Les modèles et les documentations associées fournissent des informations utiles mais n’offrent aucun garantie quant à l'origine des modèles. Une attaque peut compromettre le compte gérant le modèle ou en créer un similaire, combiné à de l’ingénierie sociale afin de compromettre la chaîne d’approvisionnement d'une application LLM.

#### 6. Adaptateurs LoRA vulnérables

LoRA est une méthode d’ajustement populaire qui permet d’ajouter des couches  intermédiaire à un LLM afin de le personnaliser. La méthode augmente l'efficacité du modèle mais crée de nouveaux risques lorsqu'un adaptateur LorA malveillant compromet l'intégrité et la sécurité du modèle de base pré-entraîné. Cela peut se produire à la fois dans des environnements de gestion des modèles, mais aussi en exploitant la prise en charge de LoRA par des plateformes populaires de déploiement d'inférence telles que vLMM et OpenLLM, où les adaptateurs peuvent être téléchargés et appliqués à un modèle déployé.

#### 7. Exploitation des processus de développement collaboratifs

La fusion collaborative de modèles et les services de manipulation de modèles (par exemple, les conversions) hébergés dans des environnements partagés peuvent être exploités pour introduire des vulnérabilités dans les modèles partagés. La fusion de modèles est très populaire sur Hugging Face, les modèles fusionnés arrivant en tête du classement OpenLLM, et peut être exploitée pour contourner les révisions. De même, il a été prouvé que des services tels que les robots de conversation sont vulnérables à la manipulation afin d'introduire des codes malveillants dans les modèles.

#### 8. Vulnérabilités liées aux chaines d'approvisionnement des LLM embarqués

Les LLM embarqués augmentent la surface d’attaque de type chaine d'approvionnement au travers de processus de fabrication compromis ou l'exploitation des failles dans les systèmes d’exploitation ou les firmwares des appareils afin de compromettre les modèles. Une attaque de ce type consiste à retro-analiser puis repackager une application avec des modèles altérés.

#### 9. Conditions générales et politiques de confidentialité floues

Des CGU et politiques de confidentialité peu claires peuvent mener à une utilisation non souhaitée des données sensibles de l’application pour l’apprentissage de modèle, pouvant entrainer une potentielle exposition de celles-ci. Cela peut également s'appliquer aux risques liés à l'utilisation, par le fournisseur du modèle, d'élements protégés par des droits d'auteur par le fournisseur du modèle.

### Stratégies de prévention et d’atténuation

1. Vérifier soigneusement les sources de données et les fournisseurs, y compris leurs CGU et politiques de confidentialité. Utiliser uniquement des fournisseurs de confiance. Auditer régulièrement leur posture de sécurité et leurs accès.
2. Appliquer les recommandations du Top 10 OWASP "A06:2021 – Vulnerable and Outdated Components" : analyse de vulnérabilités, gestion et correction. Appliquer ces mesures aussi dans les environnements de développement exposés à des données sensibles.
3. Réaliser des évaluations approfondies (AI Red Teaming) lors de la sélection de modèles tiers. Utiliser des benchmarks de confiance comme Decoding Trust. Évaluer les modèles dans leurs cas d’usage concrets.
4. Maintenir un inventaire à jour des composants avec une nomenclature logicielle (SBOM). Cela permet de prévenir la falsification, détecter rapidement les vulnérabilités (même 0-day) et garantir l'intégrité via des signatures. Explorer OWASP CycloneDX, BOMs pour IA et ML.
5. Pour réduire les risques liés aux licences, inventorier tous les types de licences via des BOMs, réaliser des audits réguliers et former les équipes aux modèles de licences. Utiliser des outils automatisés comme [Dyana](https://github.com/dreadnode/dyana) pour l’analyse dynamique des logiciels tiers.
6. Utiliser uniquement des modèles de sources vérifiables et appliquer des vérifications d’intégrité (signatures, hachages). Utiliser également la signature de code pour les composants externes.
7. Implémenter une surveillance stricte des environnements collaboratifs. Des scripts automatisés comme "HuggingFace SF\_Convertbot Scanner" peuvent aider à détecter les abus.
8. Intégrer des tests de détection d’anomalies et de robustesse adversariale dans les pipelines de MLOps et LLM, pour détecter empoisonnements ou falsifications. Ces tests peuvent aussi être menés lors d’exercices de Red Teaming.
9. Mettre en place une politique de mise à jour régulière des composants vulnérables ou obsolètes. S’assurer que l’application repose sur des API et modèles maintenus.
10. Chiffrer les modèles déployés en périphérie avec des contrôles d’intégrité. Utiliser des APIs d’attestation des fournisseurs pour bloquer les modèles ou firmwares non reconnus.

### Scénarios d’attaque

#### Scénario #1 : Librairie Python vulnérable

Une personne simule une attaque en exploitant une librairie Python vulnérable afin de compromètre une application LLM. Cela est arrivé lors de la première fuite de données d’OpenAI. Des attaques sur le registre des paquets PyPi ont incité l'équipe de développement des modèles à télécharger une dépendance PyTorch compromise avec des logiciels malveillants. Un exemple plus sophistiqué de ce type d'attaque est l'attaque Shadow Ray dans le framework Ray AI utilisé par de nombreux fournisseurs pour gérer leurs infrastructures IA. Dans cette attaque, cinq vulnérabilités auraient été exploitées, affectant de nombreux serveurs.

#### Scénario #2 : Altération directe

Une personne simule une attaque en altérant directement un modèle et en le publiant afin de diffuser de la désinformation. Exemple réel : PoisonGPT qui a contourné les contrôles de sécurité de Hugging Face en modifiant directement les paramètres du modèle.

#### Scénario #3 : Ajustement d’un modèle populaire

Un modèle populaire est ajusté pour supprimer des mécanismes de sécurité tout en gardant de bonnes performances dans un domaine spécifique (réassurance). Le modèle est ajusté pour obtenir des résultats élevés en matière de test de sécurité, mais ses déclencheurs d'attaque sont très ciblés. Puis le modèle est déployé, par exemple sur Hugging Face, pour que les victimes l'utilisent en exploitant leur confiance dans analyses comparatives de référence.

#### Scénario #4 : Modèles pré-entraînés

Un système LLM déploie des modèles pré-entraînés compromis sans vérification suffisante. Le modèle compromis introduit un code malveillant, provoquant des sorties biaisées dans certains contextes et conduisant à des résultats nuisibles ou manipulés.

#### Scénario #5 : Compromission de fournisseurs tiers

Un fournisseur tiers compromis fournit un adaptateur LorA vulnérable qui est fusionné avec un LLM à l'aide de la fusion de modèles sur Hugging Face.

#### Scénario #6 : Infiltration de fournisseur

Une personne simule une attaque en infiltrant un fournisseur tiers et compromet la production d'un adaptateur LoRA (Low-Rank Adaptation) destiné à être intégré à un LLM embarqué déployé à l'aide de frameworks tels que vLLM ou OpenLLM. L'adaptateur LoRA compromis est subtilement modifié pour inclure des vulnérabilités cachées et du code malveillant. Une fois fusionné avec le LLM, il offre à l'attaquant un point d'entrée secret dans le système. Le code malveillant peut s'activer pendant les opérations du modèle, permettant ainsi à la personne attaquante de manipuler les sorties du LLM.

#### Scénario #7 : Attaques CloudBorne et CloudJacking

Ces attaques ciblent les infrastructures cloud, en exploitant les ressources partagées et les vulnérabilités des couches de virtualisation. CloudBorne consiste à exploiter les vulnérabilités des firmware dans les environnements cloud partagés, en compromettant les serveurs physiques hébergeant les instances virtuelles. CloudJacking fait référence au contrôle malveillant ou à l'utilisation abusive d'instances cloud, ce qui peut conduire à un accès non autorisé à des plates-formes critiques de déploiement de LLM. Ces deux attaques représentent des risques importants pour les chaînes d'approvisionnement qui dépendent de modèles de ML basés sur le cloud, car les environnements compromis pourraient exposer des données sensibles ou faciliter d'autres attaques.

#### Scénario #8 : Dépassement de mémoire(CVE-2023-4969)

Exploitation par dépassement de mémoire d'une fuite de mémoire locale du GPU pour récupérer des données sensibles. Un attaquant peut utiliser cette attaque pour exfiltrer des données sensibles dans des serveurs de production et des stations de travail de développement ou des ordinateurs portables.

#### Scénario #9 : WizardLM

Suite à la suppression du modèle WizardLM, un attaquant exploite l'intérêt pour ce modèle et publie une fausse version du modèle portant le même nom mais contenant des logiciels malveillants et des portes dérobées.

#### Scénario #10 : Fusion/Conversion de modèles

Une personne simule une attaque via un service de fusion de modèles ou de conversation de format pour compromettre un modèle accessible au public afin d'injecter des logiciels malveillants. Il s'agit d'une attaque réelle publiée par le fournisseur HiddenLayer.

#### Scénario #11 : Rétro-ingénierie d'application mobile

Une personne simule une attaque en procédant à une rétro-ingénierie d'une application mobile afin de remplacer le modèle par une version altérée qui conduit l'utilisateur vers des sites frauduleux. Les utilisateurs sont encouragés à télécharger l'application directement via des techniques d'ingénierie sociale. Il s'agit d'une "véritable attaque contre des IA prédictive" qui a touché 116 applications Google Play, y compris des applications de sécurité et de sûreté très répandues, telles que la reconnaissance de billets de banque, le contrôle parental, l'authentification faciale et les services financiers. (Référence: [véritable attaque contre des IA prédictive](https://arxiv.org/abs/2006.08131))

#### Scénario #12 : Empoisonnement de jeu de données

Une personne simule une attaque en empoisonnant des jeux de données accessibles au public pour créer une porte dérobée lors de l'affinement des modèles. Cette porte dérobée favorise subtilement certaines entreprises sur différents marchés.

#### Scénario #13 : CGU et politique de confidentialité

Un opérateur de LLM modifie ses CGU pour activer par défaut l’usage des données des utilisateurs à des fins d’entraînement, entraînant une exposition de données sensibles.

### **Références**

1. [PoisonGPT: How we hid a lobotomized LLM on Hugging Face to spread fake news](https://blog.mithrilsecurity.io/poisongpt-how-we-hid-a-lobotomized-llm-on-hugging-face-to-spread-fake-news)
2. [Large Language Models On-Device with MediaPipe and TensorFlow Lite](https://developers.googleblog.com/en/large-language-models-on-device-with-mediapipe-and-tensorflow-lite/)
3. [Hijacking Safetensors Conversion on Hugging Face](https://hiddenlayer.com/research/silent-sabotage/)
4. [ML Supply Chain Compromise](https://atlas.mitre.org/techniques/AML.T0010)
5. [Using LoRA Adapters with vLLM](https://docs.vllm.ai/en/latest/models/lora.html)
6. [Removing RLHF Protections in GPT-4 via Fine-Tuning](https://arxiv.org/pdf/2311.05553)
7. [Model Merging with PEFT](https://huggingface.co/blog/peft_merging)
8. [HuggingFace SF\_Convertbot Scanner](https://gist.github.com/rossja/d84a93e5c6b8dd2d4a538aa010b29163)
9. [Thousands of servers hacked due to insecurely deployed Ray AI framework](https://www.csoonline.com/article/2075540/thousands-of-servers-hacked-due-to-insecurely-deployed-ray-ai-framework.html)
10. [LeftoverLocals: Listening to LLM responses through leaked GPU local memory](https://blog.trailofbits.com/2024/01/16/leftoverlocals-listening-to-llm-responses-through-leaked-gpu-local-memory/)

### **Frameworks et taxonomies associés**

Cette section fournit des références vers des informations complètes, des stratégies de scénarios concernant le déploiement de l'infrastructure, les contrôles appliqués à l'environnement et d'autres bonnes pratiques.

* [ML Supply Chain Compromise](https://atlas.mitre.org/techniques/AML.T0010) - **MITRE ATLAS**
