# Summary

This is a [Truss](https://truss.baseten.co/) for serving an implementation of the
[CLIP](https://github.com/openai/CLIP)(Contrastive Language-Image Pre-Training)
neural network that has been trained on image and text pairs. This allows for matching of an image with the most
relevant provided labels, without being specifically trained for that task.

Packaging this model in a Truss makes it easy to deploy it on hosting providers.

# Deploying on Baseten

To deploy this Truss on Baseten, first install the Baseten client:

```
$ pip install baseten
```

Then, in a Python shell, you can do the following to have an instance of CLIP deployed
on Baseten:

```python
import baseten
import truss

clip_handle = truss.load(".")
baseten.deploy(clip_handle, model_name="CLIP")
```

# Usage

## Inputs
The input should be a dictionary. It should contain the following:
* `labels` - a list of strings representing the labels to apply

And one of
* `image` - a 3 dimensional list in RGB representation, or
* `image_url` - a URL to of an image to be used in the model.

## Outputs

The result will be a list of dictionaries keyed by label with corresponding prediction scores.

## Example

You can invoke this model on Baseten with the following cURL command (just fill in the model version ID and API Key):

```
$ curl -X POST https://app.staging.baseten.co/model_versions/{MODEL_VERSION_ID}/predict \
    -H 'Authorization: Api-Key {YOUR_API_KEY}' \
    -d '{"image_url": "https://source.unsplash.com/gKXKBY-C-Dk/300x300", "labels": ["small cat", "not cat", "big cat"]}'
{
    "model_id": "V0NnJ0y",
    "model_version_id": "yqvx4rw",
    "model_output": {
        "status": "success",
        "data": {"predictions": [{"small cat": 0.56201171875, "not cat": 0.36865234375, "big cat": 0.06927490234375}]},
        "message": null
    }
}
```