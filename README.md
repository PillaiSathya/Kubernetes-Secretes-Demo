🔐 Kubernetes Secrets Demo

This project demonstrates how to securely inject sensitive data like username and password into a Kubernetes Pod using:

✅ Secrets as environment variables

✅ Secrets via volume mount

📁 Files Included
secret-env-pod.yaml – Pod that loads secrets as environment variables

secret-volume-pod.yaml – Pod that mounts secrets as volume files

🔧 Part 1: Secrets as Environment Variables

🛠️ 1. Create the Secret
bash
Copy
Edit
kubectl create secret generic my-secret \
  --from-literal=username=shobiuser \
  --from-literal=password=shobi@123
  
🚀 2. Apply the Pod
bash
Copy
Edit
kubectl apply -f secret-env-pod.yaml

📜 3. Check Logs
bash
Copy
Edit
kubectl logs secret-env-pod

🧾 Expected Output:

makefile
Copy
Edit
Username: shobiuser  
Password: shobi@123

✨ Why Use Secrets?
✅ Keeps sensitive data out of YAML files
🔁 Easy to rotate credentials
🔐 Best practice in enterprise DevOps

🔍 View or Decode Secrets
🔎 To View Raw Secret:
bash
Copy
Edit
kubectl get secret my-secret -o yaml
🔐 Decode (PowerShell - Windows):
powershell
Copy
Edit
[System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String("c2hvYml1c2Vy"))
[System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String("c2hvYmlAMTIz"))
Secrets are base64-encoded by default (not encrypted)"

📦 Part 2: Secrets via Volume Mount

📝 YAML: secret-volume-pod.yaml
yaml
Copy
Edit
apiVersion: v1
kind: Pod
metadata:
  name: secret-volume-pod
spec:
  containers:
    - name: app-container
      image: busybox
      command: [ "sh", "-c", "cat /etc/secret-volume/username && cat /etc/secret-volume/password && sleep 3600" ]
      volumeMounts:
        - name: secret-volume
          mountPath: "/etc/secret-volume"
          readOnly: true
  volumes:
    - name: secret-volume
      secret:
        secretName: my-secret
		
📥 Apply the Pod
bash
Copy
Edit
kubectl apply -f secret-volume-pod.yaml

📤 View Logs (if empty, use exec):
bash
Copy
Edit
kubectl logs secret-volume-pod
🧠 If logs don’t appear, try:

bash
Copy
Edit
kubectl exec -it secret-volume-pod -- cat /etc/secret-volume/username
kubectl exec -it secret-volume-pod -- cat /etc/secret-volume/password
💡 Notes on Logs
Sometimes kubectl logs may not show output due to:

⏱️ Timing issues (logs not flushed immediately)

🪟 PowerShell quirks

📦 Minimal images not logging properly

✍️ Author
Sathya – DevOps Explorer 🌐
Guided by Gemini 🤝

