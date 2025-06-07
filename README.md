## Hi there 👋
#!/bin/bash

# GitHub username
USERNAME="soumya031"

# Create a working directory
WORKDIR="./github_repos"
mkdir -p "$WORKDIR"
cd "$WORKDIR" || exit

# Clear any existing files
rm -rf ./*

echo "🔄 Fetching repository list for user: $USERNAME"
# Fetch all public repos using GitHub API
REPOS=$(curl -s "https://api.github.com/users/$USERNAME/repos?per_page=100" | jq -r '.[].clone_url')

# Clone all repos
for REPO in $REPOS; do
    echo "📥 Cloning $REPO..."
    git clone --quiet "$REPO"
done

# Initialize counters
py_count=0
js_count=0
cpp_count=0
java_count=0

# Create or clear summary
echo "📄 GitHub Repo Analysis for $USERNAME" > summary.txt
echo "----------------------------------------" >> summary.txt

# Loop through repos
for dir in */ ; do
    cd "$dir" || continue
    repo_name=$(basename "$PWD")

    # Get last commit message
    last_commit=$(git log -1 --pretty=%B 2>/dev/null)
    echo "📘 Repo: $repo_name" >> ../summary.txt
    echo "    ➤ Last commit: $last_commit" >> ../summary.txt

    # Count file types
    py_count=$((py_count + $(find . -type f -name "*.py" | wc -l)))
    js_count=$((js_count + $(find . -type f -name "*.js" | wc -l)))
    cpp_count=$((cpp_count + $(find . -type f \( -name "*.c" -o -name "*.cpp" \) | wc -l)))
    java_count=$((java_count + $(find . -type f -name "*.java" | wc -l)))

    cd ..
done

# Append totals
echo -e "\n📊 File Type Summary:" >> summary.txt
echo "    🐍 Python files: $py_count" >> summary.txt
echo "    📜 JavaScript files: $js_count" >> summary.txt
echo "    💻 C/C++ files: $cpp_count" >> summary.txt
echo "    ☕ Java files: $java_count" >> summary.txt

echo "✅ Analysis complete. Report saved to $(pwd)/summary.txt"

<!--
**soumya031/soumya031** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->
