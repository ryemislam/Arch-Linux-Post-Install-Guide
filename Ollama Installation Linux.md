# Install Ollama

```
curl -fsSL https://ollama.com/install.sh | sh
```
# Open and edit the Ollama service file

```
sudo micro /etc/systemd/system/ollama.service
```

```
[Service]
Environment="OLLAMA_HOST=0.0.0.0:11434"
```
# Reload system's service configuration and restart Ollama service

```
systemctl daemon-reload
systemctl restart ollama
```
# Run or pull various models or functions within Ollama

```
ollama pull nomic-embed-text:latest
```
```
ollama pull qwen3:14b
```
```
ollama pull deepseek-r1:14b
```
```
ollama pull gemma3:12b
```
```
ollama pull mistral-small3.2:24b
```
```
ollama pull magistral:24b
```
