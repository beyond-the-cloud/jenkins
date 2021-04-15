```bash
if [ -z '$(ls -A)' ]; then
  rm -r *
fi

gcloud container clusters get-credentials corded-terrain-309700-gke --region us-east1


# Define variables
echo "---------------------------------------------------------------------"
echo "Define variables"
echo "---------------------------------------------------------------------"

owner="beyond-the-cloud"
repo="processor-webapp-helm-chart"
GITHUB_API_TOKEN=${GITHUB_TOKEN}
GH_API="https://api.github.com"
GH_REPO="$GH_API/repos/$owner/$repo"
GH_LATEST="$GH_REPO/releases/latest"
AUTH="Authorization: token $GITHUB_API_TOKEN"

# Read asset name and id
echo "---------------------------------------------------------------------"
echo "Read asset name and id"
echo "---------------------------------------------------------------------"

response=$(curl -sH "$AUTH" $GH_LATEST)
id=`printf '%s' "$response" | jq '.assets[0] .id' |  tr -d '"'`
name=`printf '%s' "$response" | jq '.assets[0] .name' |  tr -d '"'`
GH_ASSET="$GH_REPO/releases/assets/$id"

# Print Details
echo "---------------------------------------------------------------------"
echo "Print Details"
echo "Assets Id: $id"
echo "Name: $name"
echo "Assets URL: $GH_ASSET"
echo "---------------------------------------------------------------------"

# Downloading asset file
echo "---------------------------------------------------------------------"
echo "Downloading asset file"
echo "---------------------------------------------------------------------"

curl -L -o "$name" -H "$AUTH" -H 'Accept: application/octet-stream' "$GH_ASSET"

ls -al

unzip -o "$name"

ls -al

export CHART_VERSION=$(curl -H "Authorization: token ${GITHUB_TOKEN}" "https://api.github.com/repos/beyond-the-cloud/processor-webapp-helm-chart/releases/latest" | grep '"tag_name":' | sed -E 's/.*"([^"]+)".*/\1/')
echo "CHART_VERSION: ${CHART_VERSION}"

BestIP=$(gcloud sql instances list --filter=beststories --format="value(PRIMARY_ADDRESS)")
echo $BestIP

NewIP=$(gcloud sql instances list --filter=newstories --format="value(PRIMARY_ADDRESS)")
echo $NewIP

TopIP=$(gcloud sql instances list --filter=topstories --format="value(PRIMARY_ADDRESS)")
echo $TopIP

export VAR="$(helm ls | grep processor-webapp)"

if [ "$VAR" = "" ]; then
    echo "NO RELEASE"
    helm lint processor-webapp
    helm install --set mysql.dbHost=$BestIP --set mysql.dbSchema=beststories processor-webapp-best ./processor-webapp
	helm install --set mysql.dbHost=$NewIP --set mysql.dbSchema=newstories processor-webapp-new ./processor-webapp
	helm install --set mysql.dbHost=$TopIP --set mysql.dbSchema=topstories processor-webapp-top ./processor-webapp
else
    echo "RELEASE FOUND"
    helm lint processor-webapp
    helm upgrade --set mysql.dbHost=$BestIP --set mysql.dbSchema=beststories processor-webapp-best ./processor-webapp
	helm upgrade --set mysql.dbHost=$NewIP --set mysql.dbSchema=newstories processor-webapp-new ./processor-webapp
	helm upgrade --set mysql.dbHost=$TopIP --set mysql.dbSchema=topstories processor-webapp-top ./processor-webapp
fi

rm -r *
```