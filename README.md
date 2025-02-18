
Upload videos to YouTube using geckodriver, Firefox profiles and Selenium. Easy to setup and use. Inspired by [youtube_uploader_selenium](https://github.com/linouk23/youtube_uploader_selenium).



## geckodriver
Download [geckodriver](https://github.com/mozilla/geckodriver/releases) and place it inside `C:\Users\USERNAME\AppData\Local\Programs\Python\Python37`. If you are using another version of Python, you place it inside there. This only works if Python is added to PATH and it is the correct version.

You can check if geckodriver has been added to PATH by running `geckodriver --version` in the terminal. If this gives you an error or you are running into:
### selenium.common.exceptions.WebDriverException: Message: 'geckodriver' executable needs to be in PATH.

## Configuration
Open Firefox, and go to `about:profiles`. Click "Create a New profile" and name it "Selenium" or whatever. Copy the "Root Directory" path of the new profile. This is your `root_profile_directory`. Now you can "Launch profile in new browser", go to [YouTube](https://youtube.com), and login to the account you want to upload with.

It's highly recommended that you clear your standard upload settings on YouTube.

```python
Upload(root_profile_directory: str, executable_path: str = "geckodriver", timeout: int = 3, headless: bool = True, debug: bool = True, options: FirefoxOptions = webdriver.FirefoxOptions()) -> None
```
`root_profile_directory: str` -  path to Firefox profile where you're logged into YouTube.

`executable_path: str` - full path to override which geckodriver binary to use for Firefox 47.0.1 and greater, which defaults to picking up the binary from the system path. Example: `r"C:/Users/USERNAME/Desktop/geckodriver"` (if geckodriver.exe is located in Desktop folder) Default: `geckodriver`.

`timeout: int` - seconds Selenium should wait, when getting pages and inserting data. Default: `3`.

`headless: bool` - whether or not you want to see the browser GUI. **Will override headless option if specified in `options`.** Default: `True` (hidden).

`debug: bool` - whether or not you want to see the debug info. Default: `True` (shown).

`options: FirefoxOptions` - optional options for webdriver. Use `headless` if you want to hide browser.


## Usage
```python
from Youtube-upload import Upload


upload = Upload(
    # use r"" for paths, this will not give formatting errors e.g. "\n"
    r"C:/Users/USERNAME/AppData/Roaming/Mozilla/Firefox/Profiles/r4Nd0m.selenium",
)

was_uploaded, video_id = upload.upload(
    r"C:/path/to/video.mp4",
    title="My YouTube Title",
    description="My YouTube Description",
    thumbnail=r"C:/path/to/thumbnail.jpg",
    tags=["these", "are", "my", "tags"],
    only_upload=False # If True will not set title, description or anything else. 
    # Might be useful if you want to do it manually or by using the YouTube API.
)

if was_uploaded:
    print(f"{video_id} has been uploaded to YouTube")

upload.close()
```
