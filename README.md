# Tesseract OCR Compilation - Docker Image
Create docker image having compiled tesseract with desired configuration.

This repository compiles tesseract with **--disable-openmp** configuration. Since tesseract installed using **sudo apt-get** locks when accessed simultaneously from different programs.

## Configuration
To change the configuration of tesseract compilation, change the `scripts/compile_tesseract.sh` file.

Change from:
```
./configure --disable-openmp
```
To you required configuration. e.g.
```
./configure --enable-debug
```

## Download Tessdata
For this compiled tesseract to work we have to download trained language data from [tessdata](https://github.com/tesseract-ocr/tessdata.git). This repo downloads only essential files.

To choose additional files to download add them in `scripts/tessdata_download.sh` file. 

e.g. to add **hindi** language:
```
wget -O ${TESSDATA_PREFIX}/hin.traineddata https://github.com/tesseract-ocr/tessdata/raw/4.00/hin.traineddata
```

To download all the languages:
```
git clone ${TESSDATA_REPO_URL} ${TESSDATA_PREFIX}
```
 

## Build Docker Image
1. Clone this repository.
2. `cd` into the cloned repo.
3. Run `docker build -t tesseractcmp:latest .`

## Test
1. Remove already created container with same name.
   ```
   docker stop tess_cmp
   docker rm tess_cmp
   ```
2. Run the created docker image.
   ```
   docker run -d -p 4022:22 --name tess_cmp tesseractcmp
   ```
3. Test tesseract version and list of languages available.
   ```
   docker exec -it tess_cmp tesseract \-v
   docker exec -it tess_cmp tesseract \--list-langs
   ```