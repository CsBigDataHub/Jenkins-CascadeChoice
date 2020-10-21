#!/usr/bin/env groovy
// Define variables
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