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

┌──(kali㉿kali)-[~/Downloads/btlo-spectrum]
└─$ sudo umount -r /mnt/btlo-spectrum

```

The `noise_samples.zip` file was protected with password that I managed to crack with `john`
```
┌──(kali㉿kali)-[~/Downloads/btlo-spectrum/assets]
└─$ john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
Using default input encoding: UTF-8
Loaded 1 password hash (PKZIP [32/64])
No password hashes left to crack (see FAQ)
                                                                                                                      
┌──(kali㉿kali)-[~/Downloads/btlo-spectrum/assets]
└─$ john --show hash.txt                    
noise_samples.zip:redacted_pass::noise_samples.zip:location.wav, brown.wav, wahwah.wav, white.wav:noise_samples.zip

1 password hash cracked, 0 left
```
Then we found list of `WAV` file on the protected zip file
```
┌──(kali㉿kali)-[~/Downloads/btlo-spectrum/assets]
└─$ unzip noise_samples.zip -d noise                         
Archive:  noise_samples.zip
[noise_samples.zip] brown.wav password: 
  inflating: noise/brown.wav         
  inflating: noise/location.wav      
  inflating: noise/wahwah.wav        
  inflating: noise/white.wav 
```

Use `exiftool` to extract each pictures' metadata
```
┌──(kali㉿kali)-[~/Downloads/btlo-spectrum/assets]
└─$ find ./photos|grep -E '.*\.jpe?g'|while read p;do echo;echo "Filename: $p";exiftool "$p" 2>/dev/null |tee -a ./photos/exif.txt;done 
```
By looking at the exif meta file, I found two interesting informations
```
┌──(kali㉿kali)-[~/Downloads/btlo-spectrum/assets]
└─$ cat ./photos/exif.txt|sort|uniq -c|sort -n|grep -iE 'artist|location'
      1 Artist                          : steghide password: cheese on toast
      1 Location                        : name of the challenge
```
By using the provided informations, I have managed to extract hidden stings out of `white.wav` file
```
┌──(kali㉿kali)-[~/Downloads/btlo-spectrum/assets]
└─$ steghide extract -sf ./noise/white.wav   
Enter passphrase: 
wrote extracted data to "stardate.txt".
                                                                                                                      
┌──(kali㉿kali)-[~/Downloads/btlo-spectrum/assets]
└─$ cat stardate.txt                                                     
56inrkS7AcAXatqrFM
```

Decode tool ***cyberchef***
```
docker run -it -p 8444:80 -d ghcr.io/gchq/cyberchef:latest
```
