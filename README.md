
# 7_camera_vejledning

## Slutresultat
https://user-images.githubusercontent.com/48329669/128479609-ad28c1e8-c726-4073-a961-e664b45155eb.mp4
#### Denne opgave kan ikke køres på simulator på grund af dårlig understøttelse !

## App.js
1. Start med at oprette et nyt projekt.
2. Installér følgende dependencies med ved at ligge dette ind i dine dependencies;
`  "dependencies": {
   "@react-native-community/masked-view": "^0.1.11",
   "@react-navigation/bottom-tabs": "^6.0.0",
   "react-native-gesture-handler": "^1.10.3",
   "@react-navigation/native": "^6.0.0",
   "@react-navigation/stack": "^6.0.0",
   "react-native-vector-icons": "^8.1.0",
   "expo-image-picker": "~10.2.2",
   "expo-camera": "^11.2.2",
   "expo-media-library": "^12.1.2",
   "expo": "~42.0.1",
   "expo-status-bar": "~1.0.4",
   "react": "16.13.1",
   "react-dom": "16.13.1",
   "react-native": "https://github.com/expo/react-native/archive/sdk-42.0.0.tar.gz",
   "react-native-web": "~0.13.12"
   },`
3. Opdatering: 7 okt: Hvis du installerer pakkerne med expo install, så vil der være en konflikt med kameraet aktive state, så brug disse dependecies til jeres eget projekt.

## AppStruktur
1. Opret nu følgende 2 komponenter CameraScreen og ImageScreen med "Hint 1 skabelonen" .
    1. `Husk også at komponentnavn skal være ens med filnavnet`

## App.js
1. Opret en ny Stack navigator
2. Gå ned i return og lav en Stack navigator med 2 screen komponenter - home og image. I kan evt fjerne header på home screen med headerShown false i options attributten
3. I skulle nu gerne se jeres home komponent

## CameraScreen.js
Øvelse tager udgangspunkt i https://docs.expo.dev/versions/latest/sdk/camera/ 
1. Opret en reference kaldt cameraRef. `const cameraRef = useRef(); `
2. opret nu 4 states, kaldt hasPermission, imageArr,loading og type
3. Type skal have en initial state ``Camera.Constants.Type.back``, og imageArr skal være et tomt array og permission null
4. Lav nu en useEffect funktion ( hints ) og deri en async funktion.
   1. I denne async funktion lav et object const, status, som sættes lig med `await Camera.requestPermissionAsync()`
   2. Lav nu et if statement, der tjekker tilstanden af status-variablen. Hvis status !=="granted", laves en alert som beskriver at, der ikke er givet tilladelse til brug af kamera.
   3. Dernæst laves et if statement som angiver at, Platform.OS!== "web".
   4. Hvis Platform.OS!== "web", skal der, ligesom med Camera persmisson, oprettes en const status object, der sættes ligmed `` await ImagePicker.requestMediaLibraryPermissionsAsync();``. Hvis status !=="granted", skal der igen laves en alert, der beskriver dette.
   5. Lav du en set state med setHasPermission, hvori status-variablen sættes ligmed granted
5. Derefter gå ud af useEffect og lav derefter et if statement, der tjekker betingelsen, if (hasPermission === null). Hvis denne betingelse er true, skal der returneres et tomt View
6. Hvis hasPermission derimod er false, skal der returnerew et View der wrapper hhv et tekst- og Button element. 
   1. Lav en styling til View med flex 1, så den sidder i midten
   2. I button skal du i onPress attributten lave en funktion, som kalder ``Linking.openSettings()``
7. I return-statementet oprettes en Fragment wrapper
   1. Deri vil vi øverst have et element der kaldes ``<StatusBar>`` som du kan style efter behov ( se referencer )
   2. Lav derefter et View element som har stylingen flex:1
   3. I View elementet laves et ``<Camera> </Camera>`` element, hvori attributterne type skal modtage type-statevariablen og ref skal modtage cameraRef som argument ( se referencer )
   4. Nu burde du se kameraet på din telefon
8. Vi skal nu oprette TouchableOpacities, som kan tage billeder samt gå til galleriet og vende kameraet. 
   1. I Camera elementet oprettes nu et View-elemt med en flex styling, så den udfylder hele kameraet
   2. Deri oprettes tre touchableOpacities med tekst elementer, der repræsenterer hhv Flip, Tag billede og galleri
   3. I Flip laves en funktion som skifter på Camera typen fra front til back, se evt referencer ( setType lav et inline if else statement som kigger på om det er front eller back )
   4. Test nu om du kan skifte imellem for og bag kameraet
   5. Dernæst gå ned i tag billede touchable opacitie og lav en onPress, som kalder på en snap funktion. Denne funktion skal du nu oprette
9. I snap funktionen er asynkron, idet denne indeholder et asynkront kald. 
   1. Først skal du tjekke om cameraRef.current er aktiv ``!cameraRef.current`` og hvis true returnerer ingenting
   2. Dernæst sæt loading til true
   3. Lav nu et kald `` await cameraRef.current.takePictureAsync()`` som sættes ligmed const result
   4. Såfremt du ønsker at de bileder, som du tager, skal gemmes på din enhed, kan du tilføje følgende; ``await MediaLibrary.saveToLibraryAsync(result.uri)``
   5. Dernæst skal du skal du skrive følgende for at tilføje til ImageArr ``setImagesArr((imagesArr) => [result].concat(imagesArr));``
      1. Det vi gør her er at vi laver en funktion som tager result og ligger det først i det eksisterende array, der har alle billeder
   6. Herunder sættes nu loading til false
   7. Gå ned til knappen og lav et if statement for tekst elementet med loading, hvor true skal medfører at loading eksekveres, mens false angiver, "tag billede "
   8. Test nu om dit kamera tager billeder ved at lave en console.log på result eller kig i dit galleri om du har taget et billede
10. Ved galleri knappen kald nu på en funktion kaldt pickImage og opret funktionen
    1. Funktionen skal være asynkron og tage udgangspunkt i referencen image picker i bunden
11. Husk styling til alle dine knapper evt se video øverst
12. Nu skal galleriet i image arrayet vises
    1. Opret en funktion kaldt CameraGallery som har et return
    2. I return opret nu en wrapper med View som har flex: 0.4 som styling og alignment center, padding og width på 100%
    3. I Viewet lav først et tekst element som siger antallet af billeder taget. `` imagesArr.length``
    4. Opret nu et ScrollView element som har horizontal attribute til true
    5. Dernæst lav et map funktion af imagesArr som loper igennem de billeder som er taget med TouchableOpacity som wrapper og et billedet i midten ( se evt hint)
    6. Opret også et if else som tjekker om images arr er større en 0
    7. Nu skal du kalde ``<CameraGallery/>`` i return funktionen.
    8. Tjek nu når du tager billeder at dine billeder bliver vist
    9. Hvis du har probler console.log all the way ;) 

## ImageScreen.js
1. Først opret state for image
2. lav en useEffect med en funktion som sætter route.params.image i setImage og husk at lave en return funktion for at nulstille image
3. i return funktionen lav et ``Image`` element som tage image med uri'en og null hvis der ikke er et billede. Og width + height skal bestemmes af enhedens height og width ( Dimensions.get('window'))
4. Se nu om du kan trykke på de taget billeder om de skifter view til ImagesScreen


## Ekstra opgave
1. Ekstra opgave kunne være at lave slet knapper til billederne du har taget


## Hints
### skabalon 1
``` 
const KomponentNavn = (props) => { 
    return (
        <Text>Det er mit KomponentNavn</Text>
    )
}

export default KomponentNavn; 
```
### useEffect 2
```
useEffect(() => {
        (async () => {
            
        })();
    }, []);
```

### hint 3
```
{
   imagesArr.length > 0
       ? imagesArr.map((image,index) => (
               <TouchableOpacity key={index} style={{paddingHorizontal:10}} onPress={() => navigation.navigate('image',{image:image.uri}) } >
                   <Image source={{ uri: image.uri }} style={{ width: 100, height: 200 }} />
               </TouchableOpacity>
           ))
       : <Text style={{color:"white"}}> No images taken </Text>
}

```


## Referencer
https://docs.expo.dev/versions/latest/sdk/camera/
https://docs.expo.dev/versions/latest/sdk/imagepicker/
https://reactnavigation.org/docs/stack-navigator/
https://reactnative.dev/docs/statusbar
