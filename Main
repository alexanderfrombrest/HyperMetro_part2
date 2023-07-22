package metro;
import java.io.*;
import java.util.*;
import java.util.Map;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.io.IOException;
import com.google.gson.Gson;
import com.google.gson.JsonSyntaxException;
import com.google.gson.reflect.TypeToken;
import java.util.regex.*;

public class Main {

    public static class Station {
        String name;
        Station next;
        Station prev;

        Station(String name) {
            this.name = name;
        }
    }

    public static class MetroLine {
        private String lineName;
        private Station head;
        private Station tail;


        public MetroLine(String lineName) {
            this.lineName = lineName;
        }

        public void append1(String name) {
            Station newStation = new Station(name);
            if (head == null) {
                head = newStation;
            } else {
                tail.next = newStation;
                newStation.prev = tail;
            }
            tail = newStation;
        }

        public void addhead1(String name) {
            Station newStation = new Station(name);
            if (head == null) {
                head = newStation;
                tail = newStation;
            } else {
                head.prev = newStation;
                newStation.next = head;
                head = newStation;
            }
        }

        public void remove1(String name) {
            Station current = head;

            while (current != null) {
                if (current.name.equals(name)) {
                    if (current.prev != null) {
                        current.prev.next = current.next;
                    } else {
                        head = current.next; //if current is head
                    }
                    if (current.next != null) {
                        current.next.prev = current.prev;
                    } else {
                        tail = current.prev; // if current is tail
                    }
                    return; //stops the method after station removed
                }
                current = current.next;
            }
            System.out.println("Station " + name + " does not exist");
        }

        public void output1(String lineName) {
            Station current = head;
            int i = 0;
            int count = 1;
            int x = 0;
            while (current != null && current.next != null && current.next.next != null) {
                System.out.print(current.name);
                System.out.print(" - ");
                System.out.print(current.next.name);
                System.out.print(" - ");
                System.out.println(current.next.next.name);

                current = current.next;
            }
        }

        // method to build the line from the map
        public void buildLine(Map<String, String> metroMap) {
            // Assume the station numbers in the map are sorted in the order they should be in the list
            List<String> sortedKeys = new ArrayList<>(metroMap.keySet());
            Collections.sort(sortedKeys, new Comparator<String>() {
                @Override
                public int compare(String o1, String o2) {
                    return Integer.valueOf(o1) - Integer.valueOf(o2);
                }
            });

            for (String key : sortedKeys) {
                append1(metroMap.get(key));
            }
        }
    }

    public static class MetroNetwork {
        private Map<String, MetroLine> lines;

        public MetroNetwork() {
            lines = new HashMap<>();
        }

        public void addLine(String lineName, Map<String, String> metroMap) {
            MetroLine newLine = new MetroLine(lineName);
            newLine.buildLine(metroMap);
            lines.put(lineName, newLine);
        }

        public void append(String lineName, String stationName) {
            MetroLine line = lines.get(lineName);
            if (line == null) {
                line = new MetroLine(lineName);
                lines.put(lineName, line);
            }
            line.append1(stationName);
        }

        public void addhead(String lineName, String stationName) {
            MetroLine line = lines.get(lineName);
            if (line == null) {
                line = new MetroLine(lineName);
                lines.put(lineName, line);
            }
            line.addhead1(stationName);
        }

        public void remove(String lineName, String stationName) {
            MetroLine line = lines.get(lineName);
            if (line == null) {
                System.out.println("This line doesn't exist");
            } else {
                line.remove1(stationName);
            }
        }

        public void output(String lineName) {
            MetroLine line = lines.get(lineName);
            if (line == null) {
                System.out.println("Line " + lineName + " doesn't exist.");
            }
            line.output1(lineName);
        }

        public void exit() {

        }
    }

    public static void main(String[] args) {
        String filePath = "C:/Users/Aleksander/IdeaProjects/HyperMetro1/HyperMetro/task/test/baltimore.json"; //args[0]; //"C:/Users/Aleksander/IdeaProjects/HyperMetro1/HyperMetro/task/src/metro/lausanneMetro.json"

        //create Gson object
        Gson gson = new Gson();

        //scanner
        Scanner scanner = new Scanner(System.in);
        //create MetroNetwork
        MetroNetwork network = new MetroNetwork();

        try {
            //read json file
            Reader reader = Files.newBufferedReader(Paths.get(filePath));

            //convert json file to Map
            Map<String, Map<String, String>> metroMap = gson.fromJson(reader, new TypeToken<Map<String, Map<String, String>>>() {
            }.getType());

            //close reader
            reader.close();

            //add metro line from Map to MetroNetwork
            if (metroMap != null) {
                for (Map.Entry<String, Map<String, String>> entry : metroMap.entrySet()) {
                    network.addLine(entry.getKey(), entry.getValue());
                }
            } else {
                System.out.println("The JSON file is empty");
            }

        } catch (IOException | JsonSyntaxException e) {
            if (e instanceof IOException) {
                System.out.println("Error! Such a file doesn't exist!");
            } else if (e instanceof JsonSyntaxException) {
                System.out.println("Error! Incorrect file format!");
            }
            return;
        }


        //Command loop
        while (true) {
            System.out.print("Enter command: ");
            String command = scanner.nextLine().trim();
            String[] splited = command.split("\\s+");

            if(splited[0].equals("/exit")){
                break;

            } else if (splited[0].equals("/append")) {
                String lineName = splited[1];
                String stationName = splited[2];
                network.append(lineName, stationName);

            } else if (splited[0].equals("/add-head")) {
                String lineName = splited[1];
                String stationName = splited[2];
                network.addhead(lineName, stationName);

            } else if (splited[0].equals("/remove")) {
                String lineName = splited[1];
                String stationName = splited[2];
                network.remove(lineName, stationName);

            } else if (splited[0].equals("/output")) {
                String lineName = splited[1];
                network.output(lineName);
            } else {
                System.out.print("Invalid command");
            }
        }
        scanner.close();
    }
}


// observations:
// if line not exist, nothing can be added
