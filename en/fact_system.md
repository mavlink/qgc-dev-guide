# Fact System

The Fact System provides a set of capabilities which standardizes and simplifies the creation of the QGC user interface.

## Fact

A Fact represents a single value within the system.

## FactMetaData

There is `FactMetaData` associated with each fact. It provides details on the Fact in order to drive automatic user interface generation and validation.

## Fact Controls

A Fact Control is a QML user interface control which connects to a Fact and it's `FactMetaData` to provide a control to the user to modify/display the value associated with the Fact.

## FactGroup