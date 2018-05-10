
import java.io.File;
import java.io.IOException;
import java.util.Iterator;

import org.apache.poi.EncryptedDocumentException;
import org.apache.poi.openxml4j.exceptions.InvalidFormatException;
import org.apache.poi.ss.usermodel.CellType;
import org.apache.poi.ss.usermodel.DataFormatter;
import org.apache.poi.ss.usermodel.Row;
import org.apache.poi.ss.usermodel.Sheet;
import org.apache.poi.ss.usermodel.Workbook;
import org.apache.poi.ss.usermodel.WorkbookFactory;

import com.cox.fig.util.FindingCodeUtils;

public class temp {

	public static void main(String... args)
			// readFromExcel(InputStream inputStream)
			throws EncryptedDocumentException, InvalidFormatException,
			IOException {
		Workbook workbook = WorkbookFactory.create(new File("H:\\File1.xlsx"));

		Sheet sheet = workbook.getSheetAt(0);

		DataFormatter dataFormatter = new DataFormatter();
		FindingCodeDtls currentDTO = null;
		boolean createNewDTO = true;

		Iterator<Row> rowIterator = sheet.rowIterator();
		rowIterator.next();
		while (rowIterator.hasNext()) {
			Row row = rowIterator.next();

			if (!row.getCell(0).getCellTypeEnum().equals(CellType.BLANK)) {
				// save or push currentDTO;
				FindingCodeUtils.elasticSearchCreateIndex("", "");
				// reset for next round
				createNewDTO = true;
			}
			if(createNewDTO)
			{
				currentDTO = createFindingCodesData(dataFormatter, row);
				createNewDTO = false;
			}
			currentDTO
					.addSolutionCodes(getSolutionCodesData(dataFormatter, row));
		}
		//put null checkk
		// save or push currentDTO;
		FindingCodeUtils.elasticSearchCreateIndex("", "");
	}

	private static FindingCodeDtls createFindingCodesData(
			DataFormatter dataFormatter, Row row) {
		FindingCodeDtls findingCodesDtls = new FindingCodeDtls();
		findingCodesDtls.setFindingCode(dataFormatter.formatCellValue(row
				.getCell(0)));
		findingCodesDtls
				.setScore(dataFormatter.formatCellValue(row.getCell(1)));
		findingCodesDtls.setLevel1Text(dataFormatter.formatCellValue(row
				.getCell(2)));
		findingCodesDtls.setLevel2Text(dataFormatter.formatCellValue(row
				.getCell(3)));
		findingCodesDtls.setLevel3Text(dataFormatter.formatCellValue(row
				.getCell(4)));
		findingCodesDtls.setPriority(dataFormatter.formatCellValue(row
				.getCell(5)));
		findingCodesDtls.setAutoSolutionCode(dataFormatter.formatCellValue(row
				.getCell(6)));
		findingCodesDtls.setEsrCategory(dataFormatter.formatCellValue(row
				.getCell(7)));
		findingCodesDtls.setEsrType(dataFormatter.formatCellValue(row
				.getCell(8)));
		findingCodesDtls.setEsrItem(dataFormatter.formatCellValue(row
				.getCell(9)));
		return findingCodesDtls;
	}

	private static SolutionCodes getSolutionCodesData(
			DataFormatter dataFormatter, Row row) {
		SolutionCodes solutionCode = new SolutionCodes();
		solutionCode.setSolutionCode(dataFormatter.formatCellValue(row
				.getCell(10)));
		solutionCode.setSolutionCodeDesc(dataFormatter.formatCellValue(row
				.getCell(11)));
		solutionCode.setCspp(dataFormatter.formatCellValue(row.getCell(12)));
		solutionCode.setEsp(dataFormatter.formatCellValue(row.getCell(13)));
		solutionCode.setCb(dataFormatter.formatCellValue(row.getCell(14)));
		solutionCode.setCbhl(dataFormatter.formatCellValue(row.getCell(15)));
		solutionCode.setWorkOrderAction(dataFormatter.formatCellValue(row
				.getCell(16)));
		return solutionCode;
	}
}
