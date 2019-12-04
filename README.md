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
