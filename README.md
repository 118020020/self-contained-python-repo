# About
This is a sample self-contained Python repo for my blog post [Self-Contained Python Repo](https://harrywang.me/repo).

Create `.env` file in project and put your OpenAI key as follows:

```
OPENAI_API_KEY=sk-xxxxxxxxx
```

Then, do the following and run the code using VSCode (`code .`):

```bash
$ python -m venv venv
$ source venv/bin/activate
$ pip install -r requirements.txt
```

Additional Notes:
- `pip freeze -r requirements.txt` to include the package versions.
- `train_mnist.py` is revised based on https://github.com/cloneofsimo/minDiffusion
