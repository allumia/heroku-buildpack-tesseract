# Heroku Buildpack Tesseract

This custom Heroku buildpack installs the [Tesseract OCR](https://github.com/tesseract-ocr/tesseract) binary and libraries to `/app/vendor/tesseract-ocr/`, configures the PATH for your Heroku app, and downloads custom trained OCR models from AWS S3.

## Configuration

Add this buildpack to your Heroku app:

```
heroku buildpacks:add https://github.com/allumia/heroku-buildpack-tesseract
```

### Environment Variables

**Important:** Set these environment variables on your Heroku app using `heroku config:set` to avoid storing credentials in your repository.

**Required (all three must be set or the build will fail):**
```bash
heroku config:set ML_MODELS_BUCKET_NAME=your-bucket-name
heroku config:set ML_MODELS_BUCKET_KEY_ID=your-access-key-id
heroku config:set ML_MODELS_BUCKET_KEY_SECRET=your-secret-access-key
```

**Optional:**
```bash
# Specify a specific model file to download (otherwise downloads the most recent)
heroku config:set MODEL_KEY=specific-model.traineddata

# Set AWS region if not us-east-1
heroku config:set AWS_REGION=us-west-2
```

**Security Note:** Never commit AWS credentials or sensitive information to your repository. Always use Heroku config vars.

### Installation Paths

- **Tesseract binaries:** `/app/vendor/tesseract-ocr/`
- **OCR models:** `/app/vendor/tesseract-ocr/share/tessdata/`
- **Profile script:** `/app/.profile.d/tesseract-ocr.sh`

The buildpack automatically sets `TESSDATA_PREFIX` to the models directory so Tesseract can find them.

## Features

- Installs Tesseract OCR 5.5.1 binaries to `/app/vendor/tesseract-ocr/`
- Creates a profile.d script that adds Tesseract to your app's PATH
- Downloads custom trained OCR models from AWS S3 at build time
- Requires all three ML_MODELS_* environment variables to function
- No Ruby or bundle dependencies during the build process
- Supports both specific model selection and automatic latest model download

## Usage

Once configured with the required environment variables, the `tesseract` binary will be available in your Heroku app's PATH. Your custom trained models will be automatically downloaded and available for use.

## Note

This fork upgrades the Tesseract binary version from 4.0 to 5.5.1 and adds S3 model download capabilities.

## License
MIT License.

Original work Copyright (c) 2013 Marco Azimonti

Modified work Copyright (c) 2015 Matteo Maggioni

Modified work Copyright (c) 2015 Oswell Chan

Modified work Copyright (c) 2018 Malcolm Patterson

Modified work Copyright (c) 2025 Allumia Inc