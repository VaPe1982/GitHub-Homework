# GitHub Homework – CI/CD pipeline Docker image buildhez

## A repository célja

Ez a repository egy egyszerű **CI/CD folyamat bemutatására** szolgál GitHub Actions használatával.
A pipeline feladata, hogy a repository-ba történő módosítások (push) után automatikusan:

1. felépítsen egy Docker image-et a repository-ban található **Dockerfile** alapján,
2. majd ezt az image-et feltöltse a saját **Docker Hub** repository-ba `homework:latest` taggel.

A létrehozott Docker image egy **nginx webszervert** tartalmaz, amely egy egyszerű weboldalt szolgál ki.

A weboldal megnyitásakor a következő szöveg jelenik meg:

DevOps Homework by: Varga Péter

---

## CI/CD folyamat működése

A pipeline a **GitHub Actions** segítségével valósul meg.

A folyamat lépései:

1. A fejlesztő módosítja a repository tartalmát, majd pusholja a `main` branch-re.
2. A push esemény hatására a GitHub elindítja a workflow-t.
3. A workflow egy GitHub runner környezetben fut le.
4. A pipeline letölti a repository tartalmát.
5. A Dockerfile alapján elkészíti a Docker image-et.
6. Bejelentkezik a Docker Hub fiókba (GitHub Secrets használatával).
7. Feltölti az elkészült image-et a Docker Hub repository-ba `homework:latest` taggel.

---

## Repository felépítése

A repository az alábbi fontos fájlokat tartalmazza:

### Dockerfile

Egy nginx alapú Docker image-et hoz létre, amely tartalmazza az egyedi kezdőoldalt.

### index.html

Az nginx által kiszolgált weboldal tartalma.
A weboldalon megjelenik a feladatban kért szöveg.

### .github/workflows/docker-build.yml

A GitHub Actions workflow definíciója, amely:

* buildeli a Docker image-et
* bejelentkezik a Docker Hub-ba
* feltölti az image-et a Docker Hub repository-ba

### README.md

A repository dokumentációja, amely leírja a projekt célját és működését.

---

## Docker image

A pipeline által készített Docker image elérhető a következő Docker Hub repository-ban:

https://hub.docker.com/r/mhvargapeter/homework

Az image `latest` taggel kerül publikálásra.

---

## Docker image futtatása

Az image lokálisan az alábbi parancsokkal futtatható:

docker pull mhvargapeter/homework:latest

docker run -p 8080:80 mhvargapeter/homework:latest

Ezután a böngészőben a következő címen érhető el a weboldal:

http://localhost:8080

---

## Biztonság

A Docker Hub hitelesítési adatok **nem szerepelnek a repository-ban**.

A GitHub Actions workflow a szükséges adatokat **GitHub Secrets** segítségével éri el:

* DOCKERHUB_USERNAME
* DOCKERHUB_TOKEN

Ez biztosítja, hogy a hitelesítési adatok ne kerüljenek publikusan elérhetővé.
