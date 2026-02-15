# btlo-spectrum

## Scenario
Scotland yard have intercepted information about one of the biggest drug deals to go down in the city of London. Someone we believe is linked to the deal was arrested. The only item they had in their possession was a USB thumb drive. Unfortunately, one of our junior analysts was unable to find anything of interest. Before we let this suspect go, we would like one of our DF experts to see if they can find anything about the deal before it goes down. Can you find out where and when the deal is expected to go down?

- Note: Once you have the coordinates, you can use https://www.gps-coordinates.net/ to view the location.
---

I have downloaded the evidence file into assets directory, and mounted it as a drive on my local host
```
┌──(kali㉿kali)-[~/Downloads/btlo-spectrum]
└─$ ls
684899874dd0ea2e0fce2fd52e80db72a487dda3.zip  image.dd
                                                                                                                      
┌──(kali㉿kali)-[~/Downloads/btlo-spectrum]
└─$ ls /mnt 
                                                                                                                      
┌──(kali㉿kali)-[~/Downloads/btlo-spectrum]
└─$ mkdir /mnt/btlo-spectrum                                                 
mkdir: cannot create directory '/mnt/btlo-spectrum': Permission denied
                                                                                                                      
┌──(kali㉿kali)-[~/Downloads/btlo-spectrum]
└─$ sudo mkdir /mnt/btlo-spectrum
[sudo] password for kali: 
                                                                                                                      
┌──(kali㉿kali)-[~/Downloads/btlo-spectrum]
└─$ sudo mount -o loop /home/kali/Downloads/btlo-spectrum/image.dd /mnt/btlo-spectrum 
                                                                                                                      
┌──(kali㉿kali)-[~/Downloads/btlo-spectrum]
└─$ ls /mnt/btlo-spectrum 
noise_samples.zip  photos
                                                                                                                      
┌──(kali㉿kali)-[~/Downloads/btlo-spectrum]
└─$ mkdir assets            
                                                                                                                      
┌──(kali㉿kali)-[~/Downloads/btlo-spectrum]
└─$ rsync -ravt /mnt/btlo-spectrum/ ./assets 
sending incremental file list
./
noise_samples.zip
photos/
photos/.DS_Store
photos/Millenium Bridge.jpg
photos/london-eye-1447071.jpg
photos/london_bridge.jpeg

sent 28,695,214 bytes  received 126 bytes  57,390,680.00 bytes/sec
total size is 28,687,811  speedup is 1.00

```
