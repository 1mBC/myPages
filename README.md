# Introduction
Plusieurs points d'entrée de type input file doivent être sécurisés.
Côté front, les types et tailles sont vérifiées surtout pour faciliter l'expérience utilisateur sur les limites à respecter. Il est facile de contourner ces vérifications en envoyant directement une requete http avec postman par exemple avec n'importe quel fichier et entête.

# Dispositif
- les types de fichier acceptés
    - **uploadAllowedMimeTypes** est défini pour chaque cas en dur
    - l'attribut accept={uploadAllowedMimeTypes.join(',')} dans le magicFile(s) contraint de lui même les types passant par le input, il n'est pas nécessaire de le vérifier à nouveau par script

- la taille maximale acceptée
    - **uploadMaxFileSizeMB** est défini pour chaque cas en dur (**uploadImagesMaxTotalSizeMB** dans le cas des formulaires yousign)
    - un check de taille (par fichier ou totale) est effectuée par script, surtout pour avertir l'utilisateur des limites

- customisation du fieldName dans le formData envoyé
    - même si un hackeur peut facilement repérer le fieldname de chaque input facilement, il ne pourra pas tenter de ratisser plus large avec graphql s'il n'a pas les fieldname de pages auxquelles il n'aurait pas accès

# Liste des points d'entrées

| UI | file path | formDataKey | allowedTypes | allowedSize |
|----|-----------|-------------|--------------|-------------|
|Dossier / pics | pages/dossiers/id/index | dossierDashboardPicUpload | jpeg, png | 5 |
|Admin / user / signature | components/form/UserSignature | userSignatureUpload | jpeg, png | 5 |
|Dossier / stock / saisie | components/form/saisie | files[] (see useUpload) | many | 5 |
|Dossier / audit / create (yousign) CCP - pics | components/form/CCP | NA | jpeg, png | 5 |
|Dossier / audit / create (yousign) RCG - pics | components/form/RCG | NA | jpeg, png | 5 |
|Dossier / audit / create (yousign) pdf attachements | pages/dossiers/id/audit/create/type | yousignUpload | pdf | 5 |
