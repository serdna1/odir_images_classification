## Procesado del fichero de anotaciones
El fichero 'ODIR-5K_Training_Annotations(Updated)_V2.xlsx' proporcionado junto con las imágenes me parece que tiene un formato poco amigable, así que lo procesé dando lugar a 'odir_classes.csv'. En este último cada fila contiene la información necesaria de cada una de las imágenes (el filename y la clase a la que pertenece del 0 al 7, a veces son varias clases por lo tanto varias filas).

## Preparando el dataset
Se usa la clase ImageFolder para crear el dataset. Por ello las imágenes se han estructurado siguiendo el siguiente esquema (el formato aceptado por esta clase):

- train
  - A
  - C
  - D
  - G
  - H
  - M
  - N
  - O
- val
  - A
  - C
  - D
  - G
  - H
  - M
  - N
  - O

donde cada uno de los directorios de clase contiene imágenes correspondientes a esa clase. Los caracteres hacen referencia a las siguientes enfermedades:

- N -> 'normal fundus'
- D -> 'proliferative'
- G -> 'glaucoma'
- C -> 'cataract'
- A -> 'age-related macular degeneration'
- H -> 'hypertensive'
- M -> 'myopia'
- O -> Ninguna de las anteriores

El 25% de las fotografías proporcionadas han ido a validación y el otro 75% a entrenamiento de forma randomizada.

## Entrenamiento
Se ha refinado sobre una resnet50 preentrenada en imagenet.

Transformaciones:
```
data_transforms = {
    'train': transforms.Compose([
        transforms.RandomResizedCrop(224),
        transforms.RandomHorizontalFlip(),
        transforms.ToTensor(),
        transforms.Normalize([0.485, 0.456, 0.406], [0.229, 0.224, 0.225])
    ]),
    'val': transforms.Compose([
        transforms.Resize(256),
        transforms.CenterCrop(224),
        transforms.ToTensor(),
        transforms.Normalize([0.485, 0.456, 0.406], [0.229, 0.224, 0.225])
    ]),
}
```

Parámetros de entrenamiento:
```
model_ft = models.resnet50(pretrained=True)
# Here the size of each output sample is set to 8.
# Alternatively, it can be generalized to nn.Linear(num_ftrs, len(class_names)).
num_ftrs = model_ft.fc.in_features

model_ft.fc = nn.Linear(num_ftrs, 8)

model_ft = model_ft.to(device)

criterion = nn.CrossEntropyLoss()

# Observe that all parameters are being optimized
optimizer_ft = optim.SGD(model_ft.parameters(), lr=0.001, momentum=0.9)

# Decay LR by a factor of 0.1 every 7 epochs
exp_lr_scheduler = lr_scheduler.StepLR(optimizer_ft, step_size=7, gamma=0.1)
```
El parametro num_epochs se ha dejado por defecto en 25:
```
model_ft = train_model(model_ft, criterion, optimizer_ft, exp_lr_scheduler, num_epochs=25)
```


Para más detalles ver el fichero resnet50.ipynb

## Resultados
El fichero 'output.txt' muestra los valores training loss y validation loss a lo largo de las epochs

