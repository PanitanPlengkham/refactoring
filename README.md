#Refactoring

## WordCount

From Siravit's Readability code: https://github.com/Kuea666/Covid-19-Tracker666

###initialize() Method

In the `/src/cv19Tracker/tablePageController.java` class

https://github.com/Kuea666/Covid-19-Tracker666/blob/master/src/cv19Tracker/tablePageController.java

consider this code:
```java
void initialize() {
        ReaderStrategy strategy=new ReadNumberStrategy();
        tablePage.setStyle("-fx-background-color: #4b5a81");//set background color.
        String[] Data= strategy.reader("http://covid19.th-stat.com/api/open/today");
        ObservableList<PieChart.Data>list= FXCollections.observableArrayList();
        int dataRecovered =  Integer.parseInt(Data[1]);
        int dataDeath =  Integer.parseInt(Data[2]);
        int dataInHospital =  Integer.parseInt(Data[3]);
        list.add(new PieChart.Data("Recovered",dataRecovered));
        list.add (new PieChart.Data("Dead", dataDeath));
        list.add(new PieChart.Data("InHospital",dataInHospital));
        charTPie.setData(list);//add list in pie chart
        charTPie.setStartAngle(90);//set chart start angle

        list.forEach(data ->//set number in chart
                data.nameProperty().bind(
                        Bindings.concat(
                                data.getName(), " ", data.pieValueProperty(), " "
                        )
                )
        );
    }
```
* Refactoring Signs:
  - Method is too long and complex to read   
  - It's hard code  
* Refactoring: Create Array to collect String and use ' for ' loop to reduce long code and less complex.
```java
void initialize() {
        ReaderStrategy strategy=new ReadNumberStrategy();
        tablePage.setStyle("-fx-background-color: #4b5a81");//set background color.
        String[] Data= strategy.reader("http://covid19.th-stat.com/api/open/today");
        ObservableList<PieChart.Data>list= FXCollections.observableArrayList();
        List dataName = Array.asList(new String[] {"Recovered", "Dead", "InHospital"});
        for (int i=0; i<data.length; i++) {
            list.add(new PieChart.Data(dataName[i], Integer.parseInt(Data[i+1])));
        }
        charTPie.setData(list);//add list in pie chart
        charTPie.setStartAngle(90);//set chart start angle

        list.forEach(data ->//set number in chart
                data.nameProperty().bind(
                        Bindings.concat(
                                data.getName(), " ", data.pieValueProperty(), " "
                        )
                )
        );
    }
``` 

###reader() method
In the `/src/cv19Tracker/strategy/ReadTextStrategy.java` class

https://github.com/Kuea666/Covid-19-Tracker666/blob/master/src/cv19Tracker/strategy/ReadTextStrategy.java

consider this code:
```java
 public String[] reader(String urlString) {
    StringBuilder stringBuilder = new StringBuilder(); //create an object of StringBuilder
    try {
        URL url = new URL(urlString);
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(url.openStream()));
        String line;
        while ((line = bufferedReader.readLine()) != null) {
            stringBuilder.append(line);
            stringBuilder.append(System.getProperty("line.separator"));
        }
        bufferedReader.close();
    } catch (IOException e) {
        e.printStackTrace();
    }
    String s =stringBuilder.toString();
    s=s.substring(1,s.length()-2);
    String[] splitStringArray = s.split(",");

    return splitStringArray;
    }
```
* Refactoring Signs:
  - The Method is too long.It's hard to figure out what the method does.
 
* Refactoring: Extract the method to two method.  
```java
public String[] reader(String urlString) {
        createString(urlString);
        String s = null;
        s=s.substring(1,s.length()-2);
    String[] splitStringArray = s.split(",");

    return splitStringArray;
    }

    static void createString(String urlString) {
        StringBuilder stringBuilder = new StringBuilder(); //create an object of StringBuilder
        try {
            URL url = new URL(urlString);
            BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(url.openStream()));
            String line;
            while ((line = bufferedReader.readLine()) != null) {
                stringBuilder.append(line);
                stringBuilder.append(System.getProperty("line.separator"));
            }
            bufferedReader.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
        String s =stringBuilder.toString();
    }
```
