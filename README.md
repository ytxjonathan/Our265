# Our265

An effective h265 encoder. x265 is compiled in CentOS-7 g++ (GCC) 4.8.5 20150623. 

## Usage

Only linux edition is supported. When you use Our265 at the first time, the library files (libx265.a, libx265.so, libx265.so.130)  should be copied to /usr/lib.

### Encode single sequence

command line examples:

```
./x265 --preset veryslow --input PeopleOnStreet_2560x1600_30_crop.yuv --input-res 2560x1600 --ref 4 --fps 30 --frame-thread 1 --no-wpp --bitrate 10000 --no-pmode --no-pme --slices 0 --lookahead-slices 0 --psnr --tune psnr --b-adapt 1 --bframes 15 --max-tlayer 4 --keyint 5000 -o output.265
```

#### Parameters:

##### Executable Options:

-h/--help                        
Show this help text and exit.

-V/--version                    
Show version info and exit.

##### Output Options:
-o/--output <filename>           
Bitstream output file name

-D/--output-depth 8|10|12        
Output bit depth (also internal bit depth). Default 8
   
--log-level <string>          
Logging level: none error warning info debug full. Default info

--no-progress              
Disable CLI progress reports
   
--csv <filename>              
Comma separated log file, if csv-log-level > 0 frame level statistics, else one line per run
   
--csv-log-level <integer>     
Level of csv logging, if csv-log-level > 0 frame level statistics, else one line per run: 0-2
   
##### Input Options:
--input <filename>            
Raw YUV or Y4M input file name. `-` for stdin
   
--fps <float|rational>        
Source frame rate (float or num/denom), auto-detected if Y4M

--input-res WxH          
Source picture size [w x h], auto-detected if Y4M

-f/--frames <integer>            
Maximum number of frames to encode. Default all
   
--seek <integer>              
First frame to encode

##### Quality reporting metrics:

--[no-]ssim                   
Enable reporting SSIM metric scores. Default disabled

--[no-]psnr                  
Enable reporting PSNR metric scores. Default disabled

##### Profile, Level, Tier:
-P/--profile <string>            
Enforce an encode profile: main, main10, mainstillpicture
   
--level-idc <integer|float> 
Force a minimum required decoder level (as '5.0' or '50')

--[no-]high-tier              
If a decoder level is specified, this modifier selects High tier of that level

--uhd-bd                      
Enable UHD Bluray compatibility support

--[no-]allow-non-conformance            
Allow the encoder to generate profile NONE bitstreams. Default disabled

##### Threading, performance:

--pools <integer,...>         
Comma separated thread count per thread pool (pool per NUMA node), '-' implies no threads on node, '+' implies one thread per core on node

-F/--frame-threads <integer>       
Number of concurrently encoded frames. 0: auto-determined by core count
   
--[no-]wpp                 
Enable Wavefront Parallel Processing. Default enabled

--[no-]slices <integer>   
Enable Multiple Slices feature. Default 1

--[no-]pmode                  
Parallel mode analysis. Default disabled

--[no-]pme                    
Parallel motion estimation. Default disabled

--[no-]asm <bool|int|string>     
Override CPU detection. Default: auto

##### GOP structure:
--max-tlayer
max temporal layer number. For example, GOP16: --bframes 15 --max-tlayer 4; GOP8: --bframes 7 --max-tlayer 3

##### Presets:

-p/--preset <string>
Trade off performance for compression efficiency. Default medium
ultrafast, superfast, veryfast, faster, fast, medium, slow, slower, veryslow, or placebo

-t/--tune <string>  
Tune the settings for a particular type of source or situation: psnr, ssim, grain, zerolatency, fastdecode

For more parameters, you can refer to ./x265 --help.

### Encode batch sequences
HTcondor is required to run our tests. condor_test_read_file.sh is a shell scripts and sequence_file.txt contains all sequence configurations.

(1) You can change the source dir and other options in sequence_file.txt.

(2) run the command "sh condor_test_read_file.sh sequence_file.txt".

## Performance
Encoding test sequences of JCTVC CLASS-A ~ CLASS-F in medium preset:

Compared with x265, the quality of Our265 increases with **1.15dB** in average, while the bitrate saves with **30.93%** in average.

Our work is still being improved in terms of 2-pass encoding.



### Compare with x265 encoder

|  our265 vs x265      | medium in PSNR  |
| --------   | :-----:  |
| BD-PSNR(dB)|1.1503  | 
| BD-BR(%)|   -30.93   | 

|  our265 vs x265      | medium in SSIM
| --------   | :-----:  |
| BD-SSIM| 0.0187 |
| BD-BR(%)| -32.50 |


