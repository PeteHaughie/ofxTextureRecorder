# ofxTextureRecorder

The `ofxTextureRecorder` addon allows for efficient recording of textures into image sequences or video files by utilizing multiple threads and OpenGL Pixel Buffer Objects (PBOs). It supports saving image sequences in various formats and recording video using the `ofxVideoRecorder` or `HPVLib` libraries.

## Class: `ofxTextureRecorder`

### Destructor
```cpp
~ofxTextureRecorder();
```
Cleans up and stops all encoding and saving threads upon destruction.

### Methods

#### `setup(int w, int h)`
```cpp
void setup(int w, int h);
```
Initializes the recorder with a specified width and height.

#### `setup(const ofTexture &tex)`
```cpp
void setup(const ofTexture &tex);
```
Initializes the recorder using a specified OpenGL texture.

#### `setup(const ofTextureData &texData)`
```cpp
void setup(const ofTextureData &texData);
```
Initializes the recorder using texture data, which includes internal formats and other texture parameters.

#### `setup(const Settings &settings)`
```cpp
void setup(const Settings &settings);
```
Initializes the recorder using a `Settings` object, which allows for fine-tuning of pixel formats, memory usage, and folder paths.

#### `save(const ofTexture &tex)`
```cpp
void save(const ofTexture &tex);
```
Saves the current frame from the specified texture.

#### `save(const ofTexture &tex, int frame_)`
```cpp
void save(const ofTexture &tex, int frame_);
```
Saves the current frame from the specified texture with a custom frame number.

#### `stopThreads()`
```cpp
void stopThreads();
```
Stops all running threads, closes the channels, and cleans up resources related to the recorder.

#### `cleanupFramesFolder()`
```cpp
void cleanupFramesFolder();
```
Deletes all saved frame files in the frames folder.

#### `createThreads(size_t numThreads)`
```cpp
void createThreads(size_t numThreads);
```
Creates and manages the encoding threads for handling the saving of textures to disk.

#### `getBuffer()`
```cpp
ofPixels getBuffer();
```
Gets a buffer of pixels for saving an `unsigned byte` texture.

#### `getShortBuffer()`
```cpp
ofShortPixels getShortBuffer();
```
Gets a buffer of pixels for saving a `short` texture.

#### `getFloatBuffer()`
```cpp
ofFloatPixels getFloatBuffer();
```
Gets a buffer of pixels for saving a `float` texture.

#### `getHalfFloatBuffer()`
```cpp
std::vector<half_float::half> getHalfFloatBuffer();
```
Gets a buffer for half-float textures.

#### `getAvgTimeEncode()`
```cpp
uint64_t getAvgTimeEncode() const;
```
Returns the average time it takes to encode frames.

#### `getAvgTimeSave()`
```cpp
uint64_t getAvgTimeSave() const;
```
Returns the average time it takes to save frames to disk.

#### `getAvgTimeGpuDownload()`
```cpp
uint64_t getAvgTimeGpuDownload() const;
```
Returns the average time it takes to download textures from the GPU.

#### `getAvgTimeTextureCopy()`
```cpp
uint64_t getAvgTimeTextureCopy() const;
```
Returns the average time it takes to copy textures using OpenGL.

### Nested Class: `Settings`

Settings to initialize the texture recorder. Use to define width, height, pixel format, image format, OpenGL type, and more.

#### Constructors
```cpp
Settings(int w, int h);
Settings(const ofTexture &tex);
Settings(const ofTextureData &texData);
```

### Nested Class: `VideoSettings` (Conditional on `OFX_VIDEO_RECORDER` or `OFX_HPVLIB`)

Settings specific to video recording, allowing for the configuration of frame rate, video codec, and more.

#### Constructors
```cpp
VideoSettings(int w, int h, float fps);
VideoSettings(const ofTexture &tex, float fps);
VideoSettings(const ofTextureData &texData, float fps);
```

## Dependencies

- **ofxVideoRecorder** (optional): Needed for video recording.
- **HPVLib** (optional): Needed for high-performance video recording using `.hpv` format.

## Threads and Channels

The `ofxTextureRecorder` addon utilizes multiple threads to handle texture downloading, encoding, and saving. Channels are used to pass pixel data between threads, ensuring efficient multi-threaded operation.

- **Encoding threads**: Handle the encoding process.
- **Saving threads**: Manage saving frames to disk.
- **Pixel Buffer Object (PBO)**: Used for asynchronous texture downloads from the GPU.

## Usage Example

```cpp
ofxTextureRecorder recorder;
recorder.setup(1920, 1080);
recorder.save(myTexture);
```

This will save the `myTexture` to disk using the recorder's settings. The texture will be downloaded from the GPU, encoded, and saved asynchronously.

```cpp
recorder.stopThreads();
```

Make sure to stop threads when you're done recording to clean up resources.

## Notes

- The recorder supports various pixel formats (`GL_UNSIGNED_BYTE`, `GL_SHORT`, `GL_FLOAT`, `GL_HALF_FLOAT`).
- For optimal performance, configure the number of threads based on system capabilities.
- Ensure you clean up the frames folder after the recording is done if using temporary storage.
