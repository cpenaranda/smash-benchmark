# Smash-Benchmark: Benchmark of compression libraries

[Smash](https://github.com/cpenaranda/smash) is a library that presents a single c++ API to use many compression libraries. This benchmark uses Smash with the different compression libraries to quickly show the results of each compression library.

## References
You can access results obtained with the [AI dataset](https://github.com/cpenaranda/AI-dataset) in this [paper](https://doi.org/10.1007/978-3-031-15471-3_21). Also, if you are using this repository in your research, you need to cite the paper:
> Peñaranda, C., Reaño, C., Silla, F. (2022). Smash: A Compression Benchmark with AI Datasets from Remote GPU Virtualization Systems. In: , et al. Hybrid Artificial Intelligent Systems. HAIS 2022. Lecture Notes in Computer Science(), vol 13469. Springer, Cham. https://doi.org/10.1007/978-3-031-15471-3_21

<details><summary>BibTeX</summary>

```
@InProceedings{penaranda2022smash,
  author="Pe{\~{n}}aranda, Cristian and Rea{\~{n}}o, Carlos and Silla, Federico",
  editor="Garc{\'i}a Bringas, Pablo and P{\'e}rez Garc{\'i}a, Hilde and Mart{\'i}nez de Pis{\'o}n, Francisco Javier and Villar Flecha, Jos{\'e} Ram{\'o}n and Troncoso Lora, Alicia and de la Cal, Enrique A. and Herrero, {\'A}lvaro and Mart{\'i}nez {\'A}lvarez, Francisco and Psaila, Giuseppe and Quinti{\'a}n, H{\'e}ctor and Corchado, Emilio",
  title="Smash: A Compression Benchmark with AI Datasets from Remote GPU Virtualization Systems",
  booktitle="Hybrid Artificial Intelligent Systems",
  year="2022",
  publisher="Springer International Publishing",
  address="Cham",
  pages="236--248",
  abstract="Remote GPU virtualization is a mechanism that allows GPU-accelerated applications to be executed in computers without GPUs. Instead, GPUs from remote computers are used. Applications are not aware of using a remote GPU. However, overall performance depends on the throughput of the underlying network connecting the application to the remote GPUs. One way to increase this bandwidth is to compress transmissions made within the remote GPU virtualization middleware between the application side and the GPU side.",
  isbn="978-3-031-15471-3"
}
```

</details>

## How to compile Smash
Smash contains external code. Compression libraries have been added as submodules. Some compression libraries can return an error if the data to compress is too small, or in other situations (e.g. If you try to compress small data, the result could be data that is larger than the original data. That is why some compression libraries prefer to return an error). For that reason, some compression libraries have been modified to remove this behavior and have been stored in local repositories. The idea is the user must decide if compressed data could be used.

An easy way to compile this repository is:

```
git clone git@github.com:cpenaranda/smash-benchmark.git
cd smash-benchmark
git submodule update --init --recursive --force
mkdir build
cd build
cmake -DCMAKE_BUILD_TYPE=Release ..
cmake --build . --config Release --target all
```

## How to run Smash
Smash benchmark is the best option if you want to discover the compression library that works best with your data. Here we can see the list of allowed arguments:

``` bash
bin/smash_benchmark -h
To run the smash benchmark:
 bin/smash_benchmark -c <library_name> -i <name_file>

Available arguments where minimum values are used by default.
The minimum values depend on compression libraries:

 -h, --help                                  Show this message

 -h, --help <library>                        Show information about a specific library

 -a, --available_libraries                   Show a list of the available libraries

 -c, --compression_library <library_name>    Library name to use in compression/decompression

 -i, --input_file <file_name>                File name to compress

 -o, --output_file <file_name>               File name where the compress data is stored

 -e, --best_effort                           Run all possible configurations of the selected library

 -n, --repetition_number <number>            Compression and decompression are done <number> times (1 by default)

 -b, --best <option> <number>                Get the <number> configurations of the selected library
                                             with the best <option>. Available options:
                                             0: Compression ratio
                                             1: Compression time
                                             2: Decompression time
                                             3: Total time

 -l, --level <number>                        Compression level to use
                                             Values depend of different libraries

 -s, --window_size <number>                  Set window size
                                             Values depend of different libraries

 -m, --mode <number>                         Specifies the mode used
                                             Values depend of different libraries

 -w, --work_factor <number>                  Controls how the compression works with repetitive data
                                             Values depend of different libraries

 -f, --flags <number>                        Flags to use
                                             Values depend of different libraries

 -r, --back_reference_bits <number>          Number of bits used for back-reference
                                             Values depend of different libraries

 -t, --threads <number>                      Threads used in algorithms
                                             Values depend of different libraries
```
<p>
<details><summary>Running example</summary>

``` bash
bin/smash_benchmark -c all -i Makefile
| Library    | Level | Window | Mode           | WF  | Flags         | Threads | BR  | Original data | Packed data   | Ratio  | Compress    | Decompress  | Total       |
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------
| zstd       | 1     | ------ | -------------- | --- | ------------- | ------- | --- | 121832 Bytes  | 10326 Bytes   | 11.80  | 579.92 MB/s | 123.24 MB/s | 414.57 MB/s |
| zpaq       | 0     | ------ | -------------- | --- | ------------- | ------- | --- | 121832 Bytes  | 12214 Bytes   | 9.97   | 54.72 MB/s  | 14.72 MB/s  | 39.86 MB/s  |
| zling      | 0     | ------ | -------------- | --- | ------------- | ------- | --- | 121832 Bytes  | 8565 Bytes    | 14.22  | 23.70 MB/s  | 11.55 MB/s  | 20.71 MB/s  |
| zlib-ng    | 0     | ------ | -------------- | --- | ------------- | ------- | --- | 121832 Bytes  | 121848 Bytes  | 1.00   | 2826.07 MB/s| 7304.16 MB/s| 2037.60 MB/s|
| zlib       | 0     | ------ | -------------- | --- | ------------- | ------- | --- | 121832 Bytes  | 121848 Bytes  | 1.00   | 1909.68 MB/s| 2146.50 MB/s| 1010.53 MB/s|
| z3lib      | ----- | ------ | -------------- | --- | None          | ------- | --- | 121832 Bytes  | 10273 Bytes   | 11.86  | 18.59 MB/s  | 49.92 MB/s  | 18.03 MB/s  |
| yalz77     | 1     | 10     | -------------- | --- | ------------- | ------- | --- | 121832 Bytes  | 15144 Bytes   | 8.04   | 298.31 MB/s | 174.43 MB/s | 246.02 MB/s |
| xpack      | 1     | ------ | -------------- | --- | ------------- | ------- | --- | 121832 Bytes  | 11725 Bytes   | 10.39  | 223.85 MB/s | 106.56 MB/s | 186.21 MB/s |
| wflz       | 0     | ------ | -------------- | --- | ------------- | ------- | --- | 121832 Bytes  | 16436 Bytes   | 7.41   | 0.56 MB/s   | 154.80 MB/s | 0.56 MB/s   |
| ucl        | 1     | ------ | NRV2B          | --- | ------------- | ------- | --- | 121832 Bytes  | 13015 Bytes   | 9.36   | 71.79 MB/s  | 56.59 MB/s  | 63.22 MB/s  |
| snappy     | ----- | ------ | -------------- | --- | ------------- | ------- | --- | 121832 Bytes  | 16994 Bytes   | 7.17   | 1182.93 MB/s| 407.33 MB/s | 841.89 MB/s |
| quicklz    | ----- | ------ | -------------- | --- | ------------- | ------- | --- | 121832 Bytes  | 15344 Bytes   | 7.94   | 747.86 MB/s | 167.36 MB/s | 478.54 MB/s |
| pithy      | 0     | ------ | -------------- | --- | ------------- | ------- | --- | 121832 Bytes  | 15302 Bytes   | 7.96   | 1267.01 MB/s| 359.63 MB/s | 878.35 MB/s |
| ms         | ----- | ------ | Lznt1          | --- | ------------- | ------- | --- | 121832 Bytes  | 22730 Bytes   | 5.36   | 58.34 MB/s  | 226.94 MB/s | 55.67 MB/s  |
| miniz      | 1     | 10     | None           | --- | ------------- | ------- | --- | 121832 Bytes  | 12949 Bytes   | 9.41   | 392.96 MB/s | 51.16 MB/s  | 216.35 MB/s |
| lzsse      | 1     | ------ | LZSSE2         | --- | ------------- | ------- | --- | 121832 Bytes  | 15083 Bytes   | 8.08   | 6.62 MB/s   | 571.56 MB/s | 6.61 MB/s   |
| lzo        | 0     | ------ | LZO1           | --- | ------------- | ------- | --- | 121832 Bytes  | 16067 Bytes   | 7.58   | 269.49 MB/s | 41.97 MB/s  | 145.93 MB/s |
| lzmat      | ----- | ------ | -------------- | --- | ------------- | ------- | --- | 121832 Bytes  | 12043 Bytes   | 10.12  | 31.94 MB/s  | 19.45 MB/s  | 27.48 MB/s  |
| lzma       | ----- | ------ | Default        | --- | ------------- | 1       | --- | 121832 Bytes  | 9456 Bytes    | 12.88  | 2.78 MB/s   | 4.86 MB/s   | 2.66 MB/s   |
| lzjb       | ----- | ------ | -------------- | --- | ------------- | ------- | --- | 121832 Bytes  | 20184 Bytes   | 6.04   | 294.29 MB/s | 62.31 MB/s  | 165.11 MB/s |
| lzham      | 0     | 15     | -------------- | --- | None          | ------- | --- | 121832 Bytes  | 11668 Bytes   | 10.44  | 2.50 MB/s   | 14.74 MB/s  | 2.46 MB/s   |
| lzfx       | ----- | ------ | -------------- | --- | ------------- | ------- | --- | 121832 Bytes  | 16030 Bytes   | 7.60   | 418.02 MB/s | 115.61 MB/s | 283.26 MB/s |
| lzfse      | ----- | ------ | -------------- | --- | ------------- | ------- | --- | 121832 Bytes  | 10318 Bytes   | 11.81  | 76.88 MB/s  | 111.67 MB/s | 72.64 MB/s  |
| lzf        | 0     | ------ | -------------- | --- | ------------- | ------- | --- | 121832 Bytes  | 17887 Bytes   | 6.81   | 466.77 MB/s | 202.74 MB/s | 348.85 MB/s |
| lz4        | 0     | ------ | Fast           | --- | ------------- | ------- | --- | 121832 Bytes  | 17543 Bytes   | 6.94   | 1078.04 MB/s| 599.92 MB/s | 856.43 MB/s |
| lodepng    | 1     | 10     | -------------- | 1   | Fast          | ------- | 1   | 121832 Bytes  | 17608 Bytes   | 6.92   | 82.16 MB/s  | 66.01 MB/s  | 69.64 MB/s  |
| lizard     | 0     | ------ | FastLZ4        | --- | ------------- | ------- | --- | 121832 Bytes  | 17726 Bytes   | 6.87   | 816.38 MB/s | 486.78 MB/s | 656.25 MB/s |
| liblzg     | 1     | ------ | -------------- | --- | ------------- | ------- | --- | 121832 Bytes  | 17106 Bytes   | 7.12   | 1.58 MB/s   | 132.05 MB/s | 1.58 MB/s   |
| libdeflate | 0     | ------ | Deflate        | --- | ------------- | ------- | --- | 121832 Bytes  | 121842 Bytes  | 1.00   | 5622.67 MB/s| 10148.43 MB/s| 3617.98 MB/s|
| libbsc     | 1     | 10     | Bwt            | --- | None          | ------- | 4   | 121832 Bytes  | 121860 Bytes  | 1.00   | 20.16 MB/s  | 1568.04 MB/s| 19.91 MB/s  |
| heatshrink | ----- | 4      | -------------- | --- | ------------- | ------- | 3   | 121832 Bytes  | 109738 Bytes  | 1.11   | 18.54 MB/s  | 31.86 MB/s  | 12.16 MB/s  |
| gipfeli    | ----- | ------ | -------------- | --- | ------------- | ------- | --- | 121832 Bytes  | 15832 Bytes   | 7.70   | 534.60 MB/s | 96.59 MB/s  | 310.95 MB/s |
| fse        | ----- | ------ | FSE            | --- | ------------- | ------- | --- | 121832 Bytes  | 77556 Bytes   | 1.57   | 224.41 MB/s | 209.55 MB/s | 133.44 MB/s |
| flzma2     | 1     | ------ | -------------- | --- | ------------- | 1       | --- | 121832 Bytes  | 9817 Bytes    | 12.41  | 20.13 MB/s  | 14.07 MB/s  | 18.05 MB/s  |
| flz        | 1     | ------ | -------------- | --- | ------------- | ------- | --- | 121832 Bytes  | 18007 Bytes   | 6.77   | 416.19 MB/s | 78.08 MB/s  | 232.79 MB/s |
| density    | ----- | ------ | Chameleon      | --- | ------------- | ------- | --- | 121832 Bytes  | 71258 Bytes   | 1.71   | 498.68 MB/s | 541.81 MB/s | 324.17 MB/s |
| csc        | 1     | 15     | -------------- | --- | None          | ------- | --- | 121832 Bytes  | 10367 Bytes   | 11.75  | 29.93 MB/s  | 8.44 MB/s   | 22.99 MB/s  |
| c-blosc2   | 0     | ------ | -------------- | --- | None          | 1       | --- | 121832 Bytes  | 121864 Bytes  | 1.00   | 5242.57 MB/s| 7862.70 MB/s| 3145.03 MB/s|
| bzip2      | 1     | ------ | Faster         | 0   | ------------- | ------- | --- | 121832 Bytes  | 8365 Bytes    | 14.56  | 4.81 MB/s   | 2.50 MB/s   | 4.25 MB/s   |
| brotli     | 0     | 10     | Generic        | --- | ------------- | ------- | --- | 121832 Bytes  | 40594 Bytes   | 3.00   | 21.71 MB/s  | 14.13 MB/s  | 14.36 MB/s  |
| brieflz    | 1     | ------ | -------------- | --- | ------------- | ------- | --- | 121832 Bytes  | 13180 Bytes   | 9.24   | 89.72 MB/s  | 14.84 MB/s  | 54.24 MB/s  |
```
</details>
</p>

## Different options available
There are different options in Smash, but compression libraries use only some of them. Here is the list of all the available options in Smash:

| Option              | Description    |
| :---:               | :---:          |
| Compression level   | Depending on this value, compression library gets better or faster compression                 |
| Window size         | Number of bits to indicate the window size where previous uncompressed data is allocated       |
| Mode                | Compression libraries could implements different compression/decompression modes               |
| Work factor         | This option controls how the compression works with repetitive data                            |
| Flags               | Compression libraries could implements different compression/decompression flags               |
| Back reference bits | Number of bits used to indicate the reference to previous compressed data                      |
| Number of threads   | Number of threads used by the compression library                                              |

There is an argument that shows information about a specific library.

``` bash
bin/smash_benchmark -h c-blosc2
c-blosc2       - High performance compressor optimized for binary data
To run the smash benchmark:

 -i, --input_file <file_name>                File name to compress

 -o, --output_file <file_name>               File name where the compress data is stored

 -n, --repetition_number <number>            Compression and decompression are done <number> times (1 by default)

 -l, --level <number>                        Compression level to use
                                             Available values [0-9]
                                             [compression]

 -f, --flags <number>                        Flags to use
                                             Available values [0-2]
                                             0: None
                                             1: Byte
                                             2: Bit
                                             [compression]

 -t, --threads <number>                      Threads used in algorithms
                                             Available values [1-8]
                                             [compression/decompression]
```

## Libraries used in Smash

|     |     |     | Name |     |     |     |
| :-: | :-: | :-: | :--: | :-: | :-: | :-: |
| [brieflz](https://github.com/cpenaranda/brieflz) | [brotli](https://github.com/cpenaranda/brotli) | [bzip2](https://github.com/cpenaranda/bzip2) | [c-blosc2](https://github.com/cpenaranda/c-blosc2) | [csc](https://github.com/cpenaranda/CSC) | [density](https://github.com/cpenaranda/density) | [flz](https://github.com/cpenaranda/fastlz) |
| [flzma2](https://github.com/cpenaranda/fast-lzma2) | [fse](https://github.com/cpenaranda/FiniteStateEntropy) | [gipfeli](https://github.com/cpenaranda/gipfeli) | [heatshrink](https://github.com/cpenaranda/heatshrink) | [libbsc](https://github.com/cpenaranda/libbsc) | [libdeflate](https://github.com/cpenaranda/libdeflate) | [liblzg](https://github.com/cpenaranda/liblzg) |
| [lizard](https://github.com/cpenaranda/lizard) | [lodepng](https://github.com/cpenaranda/lodepng) | [lz4](https://github.com/cpenaranda/lz4) | [lzf](https://github.com/cpenaranda/liblzf) | [lzfse](https://github.com/cpenaranda/lzfse) | [lzfx](https://github.com/cpenaranda/lzfx/) | [lzham](https://github.com/cpenaranda/lzham_codec_devel) |
| [lzjb](https://github.com/cpenaranda/lzjb) | [lzma](https://github.com/cpenaranda/xz) | [lzmat](https://github.com/cpenaranda/lzmat) | [lzo](https://github.com/cpenaranda/lzo) | [lzsse](https://github.com/cpenaranda/LZSSE) | [miniz](https://github.com/cpenaranda/miniz) | [ms](https://github.com/cpenaranda/ms-compress) |
| [pithy](https://github.com/cpenaranda/pithy) | [quicklz](https://github.com/cpenaranda/quicklz) | [snappy](https://github.com/cpenaranda/snappy) | [ucl](https://github.com/cpenaranda/ucl) | [wflz](https://github.com/cpenaranda/wflz) | [xpack](https://github.com/cpenaranda/xpack) | [yalz77](https://github.com/cpenaranda/yalz77) |
| [z3lib](https://github.com/cpenaranda/z3lib) | [zlib](https://github.com/cpenaranda/zlib) | [zlib-ng](https://github.com/cpenaranda/zlib-ng) | [zling](https://github.com/cpenaranda/libzling) | [zpaq](https://github.com/cpenaranda/zpaq) | [zstd](https://github.com/cpenaranda/zstd) | |
