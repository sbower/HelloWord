// Here is a sample stage so we can see it in our pipeline view
stage name: 'Start Sample Stage'
// Let's do a pipeline groovy call to print to screen (no node needed.)
echo 'Hello World from pipeline!'

// Must always start with a node to do any real/node logic.
node() {
    // Checkout the code from SCM (if using the same repo as the Jenkinsfile, we can use the keyword scm)
    stage name: 'Code Checkout'
    checkout scm

    // Next area of our job is to do some logic around versioning our build.
    stage name: 'Version Handling'
    def version = null
    if (binding.variables.get('RELEASE_TYPE') == 'release') {
        version = "${MAJOR}.${MINOR}.${PATCH}.${env.BUILD_NUMBER}"
    } else {
        branch = ("branch-${env.BUILD_NUMBER}-${env.BRANCH_NAME}" =~ /\\|\/|:|"|<|>|\||\?|\*|\-/).replaceAll("_")
        version = "0.0.0-${branch}.${env.BUILD_NUMBER}"
    }

    // Let's write a file to the workspace with our version information
    writeFile([file: 'version.txt', text: version])

    // Let's archive the version data file for later use.
    archive('version.txt')
}
