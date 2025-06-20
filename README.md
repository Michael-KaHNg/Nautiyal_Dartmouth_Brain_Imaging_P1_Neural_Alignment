


# 5_Sec_Alignment (Adjust Arduino Timestamp to GPIO)
[Example file:  DS12-P6_3-S1]
# use CAIMAN environmnet

# Need 3 data files 
1. Arudino behavioral text file (.txt)
2. HDF5 file (.hdf5)
3. GPIO file (.csv, Export with "First data iterm's time" **  )
##################################################################################################
# Check Alignmnet Results

1. Make sure the data length is the same for [Length of cell] AND [GPIO_Corrected_FramesImaged]  (eg 48001) (in a 40 minute session @ 20 frames per second)
2. The [Actual number of 5s pulse] should be 480 (in a 40 minute imaging session)
3. The numbers of Start/End, ON/OFF, IN/OUT timestamps should match up

![Fig_1](https://github.com/user-attachments/assets/e441cc5e-1c54-4139-bc69-976f21b23e41)

##################################################################################################
# Check that you have the right files.  The IR data (bottom) should match up between GPIO and Arduino
![Fig_2](https://github.com/user-attachments/assets/c94090c5-7026-43c2-9bd4-4766f717d705)

##################################################################################################
# The 5s pulse duration should be very close to 5s (Top) and within the red lines (bottom) 

If there are points outside the two red lines (> 10,000 ms or < 2,500 ms ), the 5s pulses are not detecting correctly.   
You will need to check the 5s pulse graph to adjust the threshold for 5s pluse detections. 

![Fig_3](https://github.com/user-attachments/assets/89543b06-55d4-4bbc-86fc-0aef0fe8c96e)

##################################################################################################
# Median of all cells. 
The POPULATION signal of neural data is usually aigned to some behavioral events. 
The reference is default to [Trial Initiation], but can be changed to others [eg. Reward Delivery]

![Fig_5](https://github.com/user-attachments/assets/a4c0e486-538f-429d-942b-c880c0a5ffba)

##################################################################################################
# Individual Cells

Individual cells are usually aigned to some behavioral events. 
The reference is default to [Trial Initiation], but can be changed to others [eg. Reward Delivery]

![Fig_6](https://github.com/user-attachments/assets/495f7c8f-fb55-40f2-91a9-c968b173f7b5)
![Fig_7](https://github.com/user-attachments/assets/4ee8d177-c37e-4777-8358-86e1c22d1646)
![Fig_8](https://github.com/user-attachments/assets/6e3ca1a7-2cf7-47c9-85f6-04ed1848a367)



##################################################################################################
##################################################################################################
# Troubleshoot Common Issues

##################################################################################################
##################################################################################################
# Wrong Hdf5 file 
Length of cells does not match length of GPIO 

![Trouble_02](https://github.com/user-attachments/assets/a74f892b-02b6-4a41-b35b-0a64084ae82c)
##################################################################################################
# Unstable Power connection (Arduino -> brain recording) disrupting BOTH the 5s Pulse and the trigger for data acqusition

During data acquisition , there will be one GPIO file, but multiple Hdf5 files because of the connection disruption

![Trouble_03](https://github.com/user-attachments/assets/5b923b70-8f16-4fc3-8591-85dda8f5fe23)

![solution_01](https://github.com/user-attachments/assets/1d0a7197-add6-4f82-813b-96a53e438c47)


SOLUTION: The program looks for the longest duration in the GPIO and assumes that's the imaging window. Uncommend this section of the code to manually set the imaging time window based on timestamps from the timestamp table (bottom). 

##################################################################################################
# Unstable Power connection (Arduino -> brain recording) causing a disrubtion in the 5s Pulse BUT data acquisition is uninterrupted 

![Trouble_04](https://github.com/user-attachments/assets/7b2b261e-c655-4f45-a256-8e3ee19801d6)

![Solution_2](https://github.com/user-attachments/assets/4fd929ed-7813-4b1a-a4b0-f698299fcbc2)

Solution:  The program has built-in repairs of the 5s pulse.  "Inter_Event_Threshold = X "  means that any "undesirable"  activities in the first X seconds of the 5s pulse would be ignore.  Check "Ka Corrected_Binary" graph to see if it gets fixed automatically.   
##################################################################################################
# Someone accidently restarted data acqusition again (Need to remove the excess data at the end)

![Trouble_05](https://github.com/user-attachments/assets/28ff4257-3810-4753-a017-ca425a9f5902)
![Solution_03](https://github.com/user-attachments/assets/f49abe2a-e974-4ed2-a9f1-86fc0aead086)

##################################################################################################

# Wrong Arduino or GPIO file

![Trouble_01](https://github.com/user-attachments/assets/97aaba34-52e0-403e-86e8-5125beadf46e)
##################################################################################################

# Arduino nummber of pulse does not match with GPIO

![Trouble_06](https://github.com/user-attachments/assets/44e9af90-6961-4a51-bec7-31c1db8a711f)

![Solution_06](https://github.com/user-attachments/assets/96fff8bd-5eef-4ad7-a02e-787c150a7c74)

Most likely the Arduino file is not printed correctly (Right file).  Sometimes duplicated information gets printed.  Just delete the extra lines in the arduino file. 



##################################################################################################
# Minor Notes For Future Alignment Scirpt
The last 5s pulse is actually an "off" pulse, don't forget to count this when programming new alignmnet script in the future

![Trouble_10](https://github.com/user-attachments/assets/3ada82a2-71cf-48b9-9543-2cb3e8f05e4b)
##################################################################################################
# Example Misaligned Data
Misaligned data will a show consistent deviation across trials (Top Right). 
Correctly aligned data ususally have cells that respond consistently to some events.  
(eg. This cell showed responses to both the trial initiation (Top Left) and to the retrieval of reward (black dot)(Bottom Left))


![Trouble_12](https://github.com/user-attachments/assets/4502bd1f-590f-403d-97fa-adc4f082a918)


