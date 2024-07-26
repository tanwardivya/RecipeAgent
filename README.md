### Recipe Agent

The Recipe Agent uses Open AI Assitants API to generate recipe for a user based on his dietary preference.

#### Prerequisite
- Python 3.11
- OS(Mac, Ubuntu)
- Docker
- Azure CLI
- Azure Account
- OpenAI API Key
- USDA Food API Key
- Mongo DB Atlas Account Key
- Postman(Optional)

#### Install
Install prerequisites mentioned above
```bash
cd backend/recipe-assistant
python3 -m venv .venv
source .venv/bin/activate
pip install -e .
```

#### Run application
```bash
./scripts/run.sh
```

#### Docker build
```docker
docker build --tag divyatanwar/agent:recipe-assistant .           
```

#### Docker Run
```docker
docker run --detach --publish 3100:3100 --env-file ../../.vscode/.env  divyatanwar/agent:recipe-assistant
```

#### Azure Container App Deployment
```az
az login
az add extension containerapp
export RESOURCE_GROUP='<RESOURCE_GROUP>' 
export LOCATION='<LOCATION>'
export CONTAINERAPPS_ENVIRONMENT='<CONTAINERAPPS_ENVIRONMENT>'
az containerapp create \
  --name recipe-agent-app \
  --resource-group $RESOURCE_GROUP \
  --environment $CONTAINERAPPS_ENVIRONMENT \
  --image divyatanwar/agent:recipe-assistant \
  --target-port 3100 \
  --ingress 'external' \
  --query properties.configuration.ingress.fqdn \
  --env-vars 'OPENAI_API_KEY'='<OPENAI_API_KEY>' 'JWT_SECRET'='<JWT_SECRET>' 'MONGODB_CONNECTION_STRING'='<MONGODB_CONNECTION_STRING>' 'OPENAI_MODEL_NAME'='gpt-4-turbo-2024-04-09'
```