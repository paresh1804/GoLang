# GoLang

As a user I need to upload a Excel file and convert it to a xml file which will then be processed in the TMS system

The source data is received from users not familar with technology/coding, hence provide data in a predefined excel format. 

Below is the code for converting csv to xml. 

package main

import (
	"encoding/csv"
	"encoding/xml"
	"io"
	"io/ioutil"
	"log"
	"os"
)

type Data struct {
	ShipmentDetails []notes `xml:"notes"`
}

type notes struct {
	Firstname string   `xml:"firstname"`
	Lastname  string   `xml:"lastname"`
	Address   *Address `xml:"address"`
}

type Address struct {
	Address1 string `xml:"address1"`
	City     string `xml:"city"`
	Country  string `xml:"country"`
}

func main() {
	csvFile, _ := os.Open("datafile.csv")
	reader := csv.NewReader(csvFile)
	var note []notes
	for {
		line, error := reader.Read()
		if error == io.EOF {
			break
		} else if error != nil {
			log.Fatal(error)
		}
		note = append(note, notes{
			Firstname: line[0],
			Lastname:  line[1],
			Address: &Address{
				Address1: line[2],
				City:     line[3],
				Country:  line[4],
			},
		})
	}
	file, _ := xml.MarshalIndent(note, "", " ")

	_ = ioutil.WriteFile("notes2.xml", file, 0644)
}
