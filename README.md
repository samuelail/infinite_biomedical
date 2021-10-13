#  Introduction

This sample IOS project was put together as a attempt to provide a practical solution to the technical challenge from my Interview with IBT.

## Question 1

To receive the Bluetooth response packets from the controller, the mobile application must:
• Parse the incoming Bluetooth packet stream (which can be considered to be an array of UInt8s) to see if a correctly formed Bluetooth packet is present
• Parse the data in the packet into a format that is usable for the mobile application
• Save the data into app memory
 
For this task, please describe how you would implement the Bluetooth receive packet process described above for an iOS application. What options you would consider for this implementation and why? What would your preferred architecture be and why?



## Solution

``` swift
//Safely unwrap characteristics value and convert to Int array

guard let data = characteristics.value else {return} 

//To see what exactly is stored in the bytes

for i in 0..<data.count {
print(data[i]) // Safely parsed UInt8 array as Int
}

//Convert from bytes to string so that it can be used within the application itself.

if let string = String(bytes: data, encoding: .utf8) {
    print(string)
    
   // Store the string in the device's storage so that it can always be called easily and quickly
    UserDefaults.standard.set(string, forKey: "device_string") 
    
    
} else {
    print("error")
}


// Call that string later to be displayed in the app
let str = UserDefaults.standard.string(forKey: "device_string") as! String
print(str)

```

## Question 2

Imagine that objects of the following type, and with the following properties, have been saved to memory using the process you designed in Prompt 1.
 
Object Name: Electrode
Object Properties:
• Index: Int
• Gain: Int
• Enabled: Bool
 
Write a class that can be used to access objects of type Electrode that have been saved to memory. Instances of this class will be created by other classes in the mobile application, such as View Controllers and View Models, and used to access the Electrode objects.
 
This class should:
• Take in some sort of parameter when initialized to filter which Electrode objects are returned. For example, this parameter could indicate that all Electrode objects should be returned, or only Electrode objects where Enabled is set to true.
• Pull all objects that match the above parameter when the class is instantiated
• If new objects that meet the above parameter are added to memory after the class is instantiated, the objects should be delivered to this class without any new actions being taken

## Solution

``` swift
    struct Electrode: Codable {
        let Index: Int
        let Gain: Int
        let Enabled: Bool
    }
    
    let electrode = Electrode(Index: 1, Gain: 3, Enabled: true)
        do {
        // Create JSON Encoder
        let encoder = JSONEncoder()

        // Encode Electrode
        let data = try encoder.encode(electrode)
       UserDefaults.standard.set(data, forKey: "Electrode")
        print("saved")

    } catch {
        print("Unable to Encode Electrode (\(error))")
    }
    
    
    
    //Get data back from memory
        if let data = UserDefaults.standard.data(forKey: "Electrode") {
        do {
            // Create JSON Decoder
            let decoder = JSONDecoder()

            // Decode Note
            let electrode = try decoder.decode(Electrode.self, from: data)
            
            //Only print electrode values if Enabled is true
            if electrode.Enabled = true {
            print(electrode)
            }
            

        } catch {
            print("Unable to Decode Electrode (\(error))")
        }
    }
        
```

