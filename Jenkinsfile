#!/usr/bin/env groovy
// Define variables
def connectorList = []
List<String> directories=["Default"]
def connectors = ""
// @Field List<String> jsonFiles = []
List<String> jsonFiles = []
jsonConnectors = ""
node {
    checkout scm
    jsonFileConnectors = findFiles(glob: '**/*.json')
    jsonFileConnectors.each { json ->
        jsonFiles << json.path
        def directory = sh(script: "echo ${json.path} | cut -d'/' -f'1'", returnStdout:true).trim()
        directories << directory
    }
    connectors = directories.unique().join(',')
    jsonConnectors = jsonFiles.unique().join(',')
}

String buildScript(List<String> values){
    List modifiedValues = values.collect{ '"' + it + '"' }
    return "return $modifiedValues"
}

String directoryValues = buildScript(directories)

println(directoryValues)

String getConnectorFiles(List<String> files){
    List modifiedFiles = files.collect{'"' + it + '"'}
    return """
    def dirs = []
    if(connectorsType.contains(',')){
                        dirs = connectorsType.split(",")
                    } else {
                        dirs.add(connectorsType)
                    }
    def jsonfiles = ${modifiedFiles}.findAll { a ->
                     dirs.any { a.contains(it) }}
    return jsonfiles
"""
}

String connectorFiles = getConnectorFiles(jsonFiles)

println(connectorFiles)

def createChoice(String name, String data){
    return [$class: 'ChoiceParameter',
            choiceType: 'PT_MULTI_SELECT',
            description: 'Select Type of Connectors',
            filterLength: 1,
            filterable: true,
            name: name,
            randomName: 'choice-parameter-603649029221735',
            script: [$class: 'GroovyScript',
                     fallbackScript: [classpath: [],
                                      sandbox: true,
                                      script: 'return ["Error"]'],
                     script: [classpath: [],
                              sandbox: true,
                              script: data]]]
}

println(createChoice("connectorsType", directoryValues))

def createCascadeChoice(String name, String refName, String script){
    return [$class: 'CascadeChoiceParameter',
            choiceType: 'PT_CHECKBOX',
            description: 'select connoectors',
            filterLength: 1,
            filterable: true,
            name: name,
            randomName: 'choice-parameter-603651102140222',
            referencedParameters: refName,
            script: [$class: 'GroovyScript',
                    fallbackScript: [classpath: [],
                                     sandbox: true,
                                     script: 'return ["error"]'],
                     script: [classpath: [],
                              sandbox: true,
                              script: script]]]
}

println(createCascadeChoice("connectorsToDeploy", "connectorsType", connectorFiles))

properties([
    parameters([createChoice("connectorsType", directoryValues),
                createCascadeChoice("connectorsToDeploy", "connectorsType", connectorFiles)
    ])
])

///////////////////////


List category_list = ['"Select:selected"','"Vegetables"','"Fruits"']
List fruits_list = ['"Select:selected"','"apple"','"banana"','"mango"']
List vegetables_list = ['"Select:selected"','"potato"','"tomato"','"broccoli"']
List default_item = ['"Not Applicable"']

String categories = buildScript(category_list)
// String vegetables = buildScript(vegetables_list)
// String fruits = buildScript(fruits_list)


String buildScript(List values){
  return "return $values"
}

String items = populateItems(default_item,vegetables_list,fruits_list)

String populateItems(List default_item, List vegetablesList, List fruitsList){
    return """
        if(SelectedCategories.equals('Vegetables')){
            return $vegetablesList
        }
        else if(SelectedCategories.equals('Fruits')){
            return $fruitsList
        }else{
            return $default_item
        }
        """
}

def createChoice(String name, String data){
       return [$class: 'ChoiceParameter', choiceType: 'PT_SINGLE_SELECT', name: name, 
        script: [$class: 'GroovyScript', fallbackScript: [classpath: [], sandbox: false, script: 'return ["ERROR"]'], script: [classpath: [], sandbox: false, script:  data]]]
}

def createCascadeChoice(String name, String refName, String script){
       [$class: 'CascadeChoiceParameter', choiceType: 'PT_SINGLE_SELECT',name: name, referencedParameters: refName, 
        script: [$class: 'GroovyScript', fallbackScript: [classpath: [], sandbox: false, script: 'return ["ERROR"]'], script: [classpath: [], sandbox: false, script: script]]]
}




properties([
    parameters([
        createChoice("SelectedCategories", categories),
        createCascadeChoice("SelectedItems", "SelectedCategories", items)
    ])
])







pipeline {
    agent any 
    stages {

        stage("first stage"){
            steps {
                echo "welcome to the ${SelectedCategories}"

            }
        
        }

        stage("Second stage"){
            steps {
                echo "bye bye world ${SelectedItems}"

            }
        
        }
    }

}
